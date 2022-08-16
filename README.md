LinearB Cycle Time Buildkite Plugin
===============================

A [Buildkite plugin](https://buildkite.com/docs/agent/plugins) to record [Deploy time](https://linearb.helpdocs.io/article/v9pckvmkbj-cycle-time) to linearB.


## Example

Add the following to your `pipeline.yaml`:

```yml
  - label: Record deployment time to linearb
    command: bin/ci_noop
    plugins:
      - cultureamp/linearb-cycle-time#v1.0.0: ~
```

To record the deploy time the [LinearB API](https://linearb.helpdocs.io/article/z4jn2k1mdj-multi-stage-delivery-api) requires the following properties:
- an api key
- event time  (epoch)
- sha
- repo url in the format `https://github.com/org/repo.git`

These values are pulled from BUILDKITE env vars or parameter store although, if you prefer some can be set directly as follows:

```yml
  - label: "linearb: record deployment time to linearb"
    command: bin/ci_noop
    plugins:
      - cultureamp/linearb-cycle-time#v1.0.0:
          api_key_ssm_param_name: "foo/bar/key"
          repo: "https://github.com/org/repo.git"
          sha: "abcdef..."
```     




