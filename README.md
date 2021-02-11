# JSON Query Wrapper

json-query-wrapper is a wrapper for the popular command-line JSON processor "[jq](https://stedolan.github.io/jq/)". This is a custom fork of [an existing composer package](https://github.com/estahn/json-query-wrapper).

## Features

* Easy to use interface
* PHP data type mapping

## Installation

```bash
$ composer require estahn/json-query-wrapper
```

## Usage
### Basic usage
`test.json`:
```json
{
  "Foo": {
    "Bar": "33"
  }
}
```

**Example 1:**
```php
$jq = JsonQueryWrapper\JsonQueryFactory::createWith('test.json');
$jq->run('.Foo.Bar'); # string(33)
```

**Example 2:**
```php
$jq = JsonQueryWrapper\JsonQueryFactory::createWith('test.json');
$jq->run('.Foo.Bar == "33"'); # Returns bool(true)
```

**Example 3:**
```php
$jq = JsonQueryWrapper\JsonQueryFactory::createWith('{"Foo":{"Bar":"33"}}');
$jq->run('.Foo.Bar == "33"'); # Returns bool(true)
```

### Advanced usage

**Example 1:**
```php
$jq = JsonQueryWrapper\JsonQueryFactory::create();
$jq->setDataProvider(new JsonQueryWrapper\DataProvider\File('test.json');
$jq->run('.Foo.Bar == "33"'); # Returns bool(true)
```

## Command Line Options

This enables passing additional command line options to `jq` to customize output and behavior. Currently, the following options are supported:

| Option | `jq` Option | Default? | Description |
| --- | --- | --- | --- |
| OPTION_EXIT_BASED_ON_OUTPUT | -e | no | set exit status based on whether output was successful |
| OPTION_INPUT_RAW | -R | no | don't parse input as JSON |
| OPTION_NULL_INPUT | -n | no | don't read any input at all |
| OPTION_OUTPUT_COLORIZE | -C | no | colorize output on shell |
| OPTION_OUTPUT_COMPACT | -c | no | compact output, don't pretty print |
| OPTION_OUTPUT_JOIN | -j | no | join each line of output, don't print newline |
| OPTION_OUTPUT_MONOCHROME | -M | yes | don't colorize output on the shell, print plainly |
| OPTION_OUTPUT_RAW | -r | no | write output directly, don't convert to JSON |
| OPTION_OUTPUT_SORT_KEYS | -S | no | sort keys in the output |

If not explicitly set, only the monochrome option will be set. More information about all these command line options can be found in the [jq manual](https://stedolan.github.io/jq/manual/#Invokingjq). To use command line options, just pass an array to the `JsonQueryFactory::create` or `JsonQueryFactory::createWith` methods:

```php
$options = [JsonQueryWrapper\CommandLineOption\CommandLineOption::OPTION_OUTPUT_JOIN, JsonQueryWrapper\CommandLineOption\CommandLineOption::OPTION_OUTPUT_SORT_KEYS];
$jq = JsonQueryWrapper\JsonQueryFactory::createWith('test.json', $options);
$jq->run('.Foo.Bar'); # string(33)
```


## Data Providers

A "Data Provider" provides the wrapper with the necessary data to read from. It's a common interface for several providers. All providers implement the `DataProviderInterface` which essentially returns a path to the file for `jq`.

Available providers:

* `Text` - Regular PHP string containing JSON data
* `File` - A path to a file containing JSON data

## Alternatives

* [jmespath.php](https://github.com/jmespath/jmespath.php) - Declaratively specify how to extract elements from a JSON document, in PHP
* [JSONPath](https://github.com/FlowCommunications/JSONPath) - JSONPath implementation for PHP (based on Stefan Goessner's JSONPath script)
