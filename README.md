LinearB Cycle Time Buildkite Plugin
===============================

A [Buildkite plugin](https://buildkite.com/docs/agent/plugins) to capture and record the time at which code changes are deployed to production. This data is posted to the linearB API and ensures accurate deploy time metrics are reported for teams and projects. 

LinearB cycle time docs can be found [here](https://linearb.helpdocs.io/article/v9pckvmkbj-cycle-time).

## Example Usage

Add the following to your `pipeline.yaml`:

```yml
  - label: "linearB: record the time of deployment"
    command: bin/ci_noop
    plugins:
      - cultureamp/linearb-cycle-time#v1.0.1: ~
```

Keep in mind that you only want to call the linearB API once and your pipeline may include multiple production deploys (US, EU). You will likely need to restrict when this plugin executes. This will depend on how the pipeline has been structured but may include branch and/or step dependancies as illustrated below:

```yaml
  - label: "linearB: record the time of deployment"
    depends_on: 
      - prod_us_deployed
      - prod_eu_deployed
    command: bin/ci_noop
    branch: main
    plugins:
      - cultureamp/linearb-cycle-time#v1.0.1: ~
```

### Modifying the default values (likely not required for most users)

To record the time at which code is deployed, the [linearB API](https://linearb.helpdocs.io/article/z4jn2k1mdj-multi-stage-delivery-api) requires the following properties:
- an api key
- event time  (epoch)
- sha
- repo url (in the format `https://github.com/org/repo.git`)

Whilst the plugin is able to pull these values from Buildkite environment variables and a default parameter store location, should it be required, these values can be overridden as follows:

```yml
  - label: "linearB: record the time of deployment"
    command: bin/ci_noop
    plugins:
      - cultureamp/linearb-cycle-time#v1.0.1:
          api_key_ssm_param_name: "foo/bar/key"
          repo: "https://github.com/org/repo.git"
          sha: "abcdef..."
```     




