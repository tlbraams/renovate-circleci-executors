# #30558

https://github.com/renovatebot/renovate/discussions/30558

## Current behavior

Docker images used when defining [executors](https://circleci.com/docs/configuration-reference/#executors) are no longer recognized as dependencies by renovate.
Before a [refactor](https://github.com/renovatebot/renovate/pull/30142) that changed the circleci manager from regex based parsing to a YAML based schema, these images were recognized by renovate.

I confirmed that this used to work by installing the version (37.429.0) before this change and executing a local run on this reproduction:
<details>
<summary>Logs</summary>

```shell
% LOG_LEVEL=debug renovate --platform=local
...
 INFO: Repository started (repository=local)
       "renovateVersion": "37.429.0"
...
DEBUG: Matched 1 file(s) for manager circleci: .circleci/config.yml (repository=local)
DEBUG: CircleCI docker image (repository=local)
       "depName": "cimg/ruby",
       "currentValue": "3.0.3-browsers",
       "currentDigest": undefined
DEBUG: manager extract durations (ms) (repository=local)
       "managers": {"circleci": 1}
DEBUG: Found circleci package files (repository=local)
DEBUG: Found 1 package file(s) (repository=local)
 INFO: Dependency extraction complete (repository=local)
       "stats": {
         "managers": {"circleci": {"fileCount": 1, "depCount": 1}},
         "total": {"fileCount": 1, "depCount": 1}
       }
...
DEBUG: packageFiles with updates (repository=local)
       "config": {
         "circleci": [
           {
             "deps": [
               {
                 "depName": "cimg/ruby",
                 "currentValue": "3.0.3-browsers",
                 "replaceString": "cimg/ruby:3.0.3-browsers",
                 "autoReplaceStringTemplate": "{{depName}}{{#if newValue}}:{{newValue}}{{/if}}{{#if newDigest}}@{{newDigest}}{{/if}}",
                 "datasource": "docker",
                 "depType": "docker",
                 "versioning": "docker",
                 "updates": [
                   {
                     "bucket": "non-major",
                     "newVersion": "3.3.4",
                     "newValue": "3.3.4-browsers",
                     "newMajor": 3,
                     "newMinor": 3,
                     "newPatch": 4,
                     "updateType": "minor",
                     "branchName": "renovate/cimg-ruby-3.x"
                   }
                 ],
                 "packageName": "cimg/ruby",
                 "warnings": [],
                 "registryUrl": "https://index.docker.io",
                 "currentVersion": "3.0.3",
                 "isSingleVersion": true,
                 "fixedVersion": "3.0.3-browsers"
               }
             ],
             "packageFile": ".circleci/config.yml"
           }
         ]
       }
...
DEBUG: Matched 1 file(s) for manager circleci: .circleci/config.yml (repository=local)
DEBUG: manager extract durations (ms) (repository=local)
       "managers": {"circleci": 5}
DEBUG: Found 0 package file(s) (repository=local)
 INFO: Dependency extraction complete (repository=local)
       "stats": {"managers": {}, "total": {"fileCount": 0, "depCount": 0}}
...
DEBUG: packageFiles with updates (repository=local)
       "config": {}
```

</details>

Installing the next available version (37.431.0) fails to detect this image as a dependency:
<details>
<summary>Logs</summary>

```shell
% LOG_LEVEL=debug renovate --platform=local
...
 INFO: Repository started (repository=local)
       "renovateVersion": "37.431.0"
...
DEBUG: Matched 1 file(s) for manager circleci: .circleci/config.yml (repository=local)
DEBUG: manager extract durations (ms) (repository=local)
       "managers": {"circleci": 5}
DEBUG: Found 0 package file(s) (repository=local)
 INFO: Dependency extraction complete (repository=local)
       "stats": {"managers": {}, "total": {"fileCount": 0, "depCount": 0}}
...
DEBUG: packageFiles with updates (repository=local)
       "config": {}
```
</details>

Installing the latest version (38.17.0) still fails to detect the image as a dependency:
<details>
<summary>Logs</summary>

```shell
...
 INFO: Repository started (repository=local)
       "renovateVersion": "38.17.0"
...
DEBUG: Matched 1 file(s) for manager circleci: .circleci/config.yml (repository=local)
DEBUG: manager extract durations (ms) (repository=local)
       "managers": {"circleci": 2}
DEBUG: Found 0 package file(s) (repository=local)
 INFO: Dependency extraction complete (repository=local)
       "stats": {"managers": {}, "total": {"fileCount": 0, "depCount": 0}}
...
DEBUG: packageFiles with updates (repository=local)
       "config": {}
```

</details>

## Expected behavior

I expect renovate to still recognize docker images listed in executors as dependencies.

## Link to the Renovate issue or Discussion

Put your link to the Renovate issue or Discussion here.
