# Export template helper
This command line tool is used to render Jinja2 templates using Rossum annotation payload and to manage settings of the Custom Export Pipeline extension.

# Installation guide
```
brew install pipx
pipx ensurepath
pipx install .
```

# User guide
There are three use cases this tool helps you with, they are listed below. They render output to stdout.

## Render Jinja2 template
Locally render Jinja2 template using annotation payload either from local file or Rossum API. See the sample commands below.

Render template using locally stored payload:
```
poetry run export-template-helper -r -t {template file path} -l {payload file path}
poetry run export-template-helper -r -t template.json -l data.json
```

Render template using data from Rossum API:
```
poetry run export-template-helper -r -t {template file path} -a {annotationId} -o {hookId} -u {baseUrl} -n {authToken}
poetry run export-template-helper -r -t template.json -a 123456 -o 123456 -u https://your-org.rossum.app/api/v1 -n 4ccf1d11a42070e70c132f2678076b412489339f 
```

In case of a local file, the full annotation payload as Rossum generates it is expected.

## Generate extension settings
Generate configuration of the Custom Export Pipeline extension using the Jinja2 template and a reference key:

```
poetry run export-template-helper -g -t template.json -k json
```

## Parse template from extension settings
Create a local template file from settings of the Custom Export Pipeline extension:

```
poetry run export-template-helper -p -t {template file path} -o {hookId} -u {baseUrl} -n {authToken}
poetry run export-template-helper -p -t template.json -o 123456 -u https://your-org.rossum.app/api/v1 -n 4ccf1d11a42070e70c132f2678076b412489339f
```

*Note that the export reference key is not stored among the template, you will need to provide it as an argument when generating the settings*

## Typical usage (tell it to me like I'm a 6yo)
* Clone this repo `git clone https://github.com/rossumai/export-template-helper`
* Install it `pipx install .`
* Add your template to the root or fetch it from the extension config: `poetry run export-template-helper -p -t {template file path} -o {hookId} -u {baseUrl} -n {authToken}`
* Note `annotation ID`, `hook ID`, `base URL` and `auth token` and run this: `poetry run export-template-helper -r -t {template file path} -a {annotationId} -o {hookId} -u {baseUrl} -n {authToken}`
* Keep editing the template until it's ready
* When the template is ready, run this: `poetry run export-template-helper -g -t {template file path} -k {export reference key}`
* Copy the configuration from stdout to extension config

# Full list of parameters
## Commands
|param|description|
|--|--|
|`-r --render`|render a local Jinja2 template with Rossum annotation payload|
|`-g --generate`|generate Custom Export Pipeline extension configuration using a local Jinja2 template|
|`-p --parse`|parse Jinja2 template from hook settings into local file|

## Parameters
|param|description|
|--|--|
|`-t --templatePath`|path to local Jinja2 template|
|`-l --payloadPath`|path to local file with Rossum annotation payload|
|`-a --annotationId`|annotation ID to fetch payload from Rossum API|
|`-n --token`|Rossum API token|
|`-o --hookId`|Rossum hook ID|
|`-u --url`|Rossum base API URL|
|`-k --key`|Export reference key|
