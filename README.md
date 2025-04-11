# gitlab-ci

Collection of gitlab ci jobs/scripts

## Job lint_renovate_config

The pipeline job `lint_renovate_config` checks whether Renovate configuration files (e.g., `renovate.json`, `config.js`) are correct and returns the results.

### Job configuration

**ENV-Variables:**
* `LRC_JOB_DISABLED`: if true, job will be deactivated. Default `false`.
* `LRC_CUSTOM_CONFIG_LIST`: List of specific configuration files for validation, separated by spaces. Wildcards (e.g. "config*.js", "*.json") are supported.
* `LRC_STRICT_MODE`: Validation error handling. It accepts the following values:
  * 0: The job won't fail even if validation warnings or errors occur. But a specific exit code '65' will be thrown at the end.
  * 1: The job fails if validation errors occur
  * 2: The job fails if either warnings or validation errors occur.

> [!IMPORTANT]
> The job will only be executed if:
> * the variable `LRC_JOB_DISABLED` is not set to `true`.
> * and either the file `renovate.json` is present in the repository or the variable `LRC_CUSTOM_CONFIG_LIST` is set.
>
> If the variable `LRC_CUSTOM_CONFIG_LIST` is set, only the files specified in the list will be validated, while the `renovate.json` file is ignored.

### Usage

```yaml
include:
  - https://url_to_job_lint_renovate_config/<version>.yaml
variables:
    LRC_CUSTOM_CONFIG_LIST: "config*.js config*.json" 
```

## Job validate_input_variables

The pipeline job `validate_input_variables` can be used to validate variables in a Gitlab CI pipeline.

Due to Gitlab CI limitations, this job is passed a variable.
This variable lists the variables to be validated, separated by *;*. It is important that each variable entry consists of the *name* followed by a space and the *value*. The value is checked for *empty string*, *0*, and *UNDEFINED*. If a match is found, an error is output with the variable name.

### Job configuration

**ENV-Variables:**
* `VIV_JOB_DISABLED`: if true, job will be deactivated. Default `false`.
* `VARIABLES_TO_VALIDATE`: Specify the variables to be checked in a variable. Each entry consists of the variable's name and value and is separated from other variables with *;*. Mandatory, no default.

### Usage

```yaml
include:
  - https://url_to_job_validate_input_variables/<version>.yaml

validate_input_variables:
  VARIABLES_TO_VALIDATE: Variable1.Name ${Variable1.Value}; Variable2.Name ${Variable2.Value}
```
