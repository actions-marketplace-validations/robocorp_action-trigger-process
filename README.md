# GitHub Action to trigger a Robocorp Control Room process

This GitHub Actions triggers a Robocorp Control Room process run and optionally waits for its execution to complete.

## Usage

### Example Workflow file

An example workflow to trigger and await a Control Room process run:

```yaml
jobs:
  run-process:
    runs-on: ubuntu-latest
    name: Trigger process
    steps:
      - name: Trigger Control Room process run
        uses: robocorp/action-trigger-process@v1
        with:
          api-key: ${{ secrets.ROBOCORP_API_KEY }}
          workspace-id: ${{ secrets.ROBOCORP_WORKSPACE_ID }}
          process-id: ${{ secrets.ROBOCORP_PROCESS_ID }}
          payload: '{"foo":"bar"}'
          await-complete: true
```

##### Configuration

| Option             | Value   | Required | Default                                 | Description                                                            |
| ------------------ | ------- | -------- | --------------------------------------- | ---------------------------------------------------------------------- |
| api-key            | string  | \*       |                                         | Workspace API key with `read_runs` and `trigger_processes` permissions |
| workspace-id       | string  | \*       |                                         | The target Control Room workspace ID                                   |
| process-id         | string  | \*       |                                         | The target Control Room process ID                                     |
| payload            | string  |          | "{}"                                    | Stringified JSON payload passed to process                             |
| await-complete     | boolean |          | false                                   | Should the action await process run completion                         |
| fail-on-robot-fail | boolean |          | true                                    | Fail the GitHub workflow run if Control Room process fails             |
| timeout            | number  |          | 120                                     | Process run await timeout in seconds                                   |
| api-endpoint       | string  |          | https://api.eu1.robocloud.eu/process-v1 | Robocorp workspace API endpoint                                        |

#### Outputs

| Name         | Value                                 | Description                                   |
| ------------ | ------------------------------------- | --------------------------------------------- |
| run-id       | string                                | Process run id                                |
| duration     | number                                | Process run execution duration                |
| robotrun-ids | string                                | Comma seperated list of process robot run IDs |
| state        | "COMPL" &#124; "ERR" &#124; "TIMEOUT" | Process run state code                        |
