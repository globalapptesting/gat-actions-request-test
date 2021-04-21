# GlobalAppTesting - Crowdtesting Github Action

Launch an exploratory test with Global App Testing and find issues impacting your users.

To get your API key sign up [here](https://go.globalapptesting.com/speak-to-us).

## Usage

To request the test you need to:
  - specify API key as an action input(example below)
  - specify github access token("repo" permissions are enough) as an action input
  - add `.gat.json` file to your repository(example below)

```yaml
# required to get .gat.json file from your repository
- uses: actions/checkout@v2
- name: Test with GAT
  uses: GlobalAppTesting/gat-actions-request-test@v1
  with:
    api_key: ${{ secrets.GAT_API_KEY }}
    access_token: ${{ secrets.MY_NEW_ACCESS_TOKEN }}
```

```js
// .gat.json
{
  "description": "Go to https://globalapptesting.com and see what you can find!"
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
  uses: GlobalAppTesting/gat-actions-request-test@v1
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
- `api_key` - **required**. GAT API key. Get one [here](https://go.globalapptesting.com/speak-to-us).
- `access_token` - **required**. Github access token we will use to export issues. "repo" permissions are enough.
- `repository` - **optional**. Repository where our testers will report found issues.

### Outputs
- None

### Reads fields from config file inside your repository's `.gat.json`:
- `description` - **required**. Can't be empty.
