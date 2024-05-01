## Demo retry GHA workflow

[test-retry-example.yaml](.github/workflows/test-retry-example.yaml)
```
name: Test
run-name: ${{ github.actor }} is testing out GitHub Actions 🚀
on: [push]
jobs:
  tests:
    runs-on: ubuntu-latest
    steps:
      - name: Random change of failure simulating flaky test
        id: run-test
        run: exit $(($RANDOM%2))
        # we want to retry the test if it fails
        continue-on-error: true
      - name: Retry test
        # this step will be skipped if the run-test step is successful
        if: ${{ steps.run-test.outcome == 'failure' }}
        run: echo "Retrying the test..."
```

## Sample Run
After a few [runs](../../actions/runs/8911328941), in the summary page we see that run-test step failed but the retry was successful
<img width="1895" alt="image" src="https://github.com/alonch/dd-ci-step-demo/assets/6455579/530b3f3d-3540-4c2f-a9da-95896bb2c9f2">

How could we monitor these step failures in Datadog?
