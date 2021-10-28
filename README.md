# GlobalAppTesting - Crowdtesting Github Action

Launch a test with real testers on real devices, and quickly uncover the highest impact issues.

You can generate your API key [here](https://app.globalapptesting.com/organization-settings/customer-api-key) or sign up [here](https://go.globalapptesting.com/early-access-exploratory-testing-test-execution).

## Usage

To request the test you need to:
  - specify API key as an action input(example below)
  - specify github access token("repo" permissions are enough) as an action input
  - add `.gat.json` file to your repository(example below)

```yaml
# required to get .gat.json file from your repository
- uses: actions/checkout@v2
- name: Test with GAT
  uses: GlobalAppTesting/gat-actions-request-test@v2
  with:
    api_key: ${{ secrets.GAT_API_KEY }}
    access_token: ${{ secrets.MY_NEW_ACCESS_TOKEN }}
    application_url: https://yourapplicationurl.com
```

```js
// .gat.json
{
  "description": "Your company's description and additional context for testers",
  "test_cases": []
}
```

And that's it! 🎉

Once you trigger the action our testers will start investigating whatever you'll ask them to. As soon as any issue will be found you'll see it reported in this repository's issues tab.

### Advanced usage

If you'd like to have issues reported in separate repository you can set it as an action input. Repository has to be accessible using **your access token**.

```yaml
# required to get .gat.json file from your repository
- uses: actions/checkout@v2
- name: GlobalAppTesting
  uses: GlobalAppTesting/gat-actions-request-test@v2
  with:
    api_key: ${{ secrets.GAT_API_KEY }}
    access_token: ${{ secrets.MY_NEW_ACCESS_TOKEN }}
    repository: owner/my-other-repo
```

### Examples

You can see how this action can be used [here](https://github.com/GlobalAppTesting/gat-action-kickstarter).

More examples can be find [here](https://github.com/GlobalAppTesting/gat-actions-examples).

----
## Action Spec

### Environment variables
- None

### Inputs
- `api_key` - **required**. GAT API key. Generate one [here](https://app.globalapptesting.com/organization-settings/customer-api-key) or sign up [here](https://go.globalapptesting.com/early-access-exploratory-testing-test-execution).
- `access_token` - **required**. Github access token we will use to export issues. "repo" permissions are enough.
- `dry_run` - **optional**. Try out the action **without actually launching the test**. Dry run will validate structure of your inputs and access to your repository. **default: false**(so you WILL launch a real test!)
- `repository` - **optional**. Repository where our testers will report found issues.
- `organization_name` - **optional**. Name of your organization to display for testers. If none given owner of the repository is used.
- `application_url` - **optional**. URL under which your application will be available for testing. If that's more convenient for you, you can also put the URL directly in the description, although this way might sooner or later get deprecated. This field is now the advised way for pointing our testers to your application.
- `wait_for_finished_testing` - **optional**. **Only useful if using this action with `on: pull_request` trigger!**. Set to true to mark your PR as pending until results from GAT testers will come back. GAT will mark your PR(using your access token) green when tests will complete successfully. If there were any test case failures GAT will open them as issues and mark your PR red until next testing is requested.
- `pr_status_context` - **optional**. **Only useful if using this action with `on: pull_request trigger!**. Github Commit Status context which will be displayed as one of the checks on your Pull Request. Currently we support 3 checks - pending(our testers has received your request and now they're testing), success(test has finished, testers found 0 issues) and failure(testers have found issues in your app). If none given, "GlobalAppTesting" will be use as default context.
- `issue_prefix` - **optional**. Prefix which will be used in the titles of reported issues. For example, `"GlobalAppTesting"` will result in issue titled like `"[GlobalAppTesting] <issue title>"`. Useful in monorepos(set `issue_prefix` based on PR title or branch, see examples section) to filter issues per package. Can also be used for simple filtering the issues reported by GAT.

### Outputs
- None

### Reads fields from config file inside your repository's `.gat.json`:
- `description` - **required**. Can't be empty.
- `test_cases` - **optional**. Array of test case instructions. For sample format see `.gat.json.example`.
