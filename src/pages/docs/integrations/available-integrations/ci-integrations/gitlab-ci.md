---
title: "GitLab CI/CD"
updated: 2022-06-09
contextual_links:
  - type: section
    name: "Prerequisites"
  - type: link
    name: "CI Integrations"
    url: "/docs/integrations/ci-integrations/"
---

> __[GitLab Self-Managed integrations are only available on Postman Enterprise plans.](https://www.postman.com/pricing)__

[GitLab CI/CD](https://docs.gitlab.com/ee/ci/) is a continuous integration (CI) and continuous delivery (CD) service that's integrated with GitLab SaaS and GitLab self-managed. Software development teams can use GitLab CI/CD to automatically build, test, and deploy code in GitLab.

To set up a GitLab CI/CD integration for your API, first create a pipeline in GitLab and then configure your API in Postman. After you set up the integration, you can view the status of builds or start a new build, all from within Postman.

> If your GitLab pipeline is configured to run API tests using [Newman](/docs/running-collections/using-newman-cli/command-line-integration-with-newman/), you can [configure the Postman cloud reporter](#configuring-newman-for-gitlab-cicd) to send collection run information back to Postman.

## Creating a pipeline in GitLab

If you haven't already, create a pipeline in the GitLab repository you use for your API. To create a pipeline, add a `.gitlab-ci.yml` file in the root of your repository. You define your pipeline in this YAML file. To learn more, see [the GitLab CI/CD documentation](https://docs.gitlab.com/ee/ci/).

## Configuring a GitLab CI/CD integration

To configure a GitLab CI/CD integration for Gitlab SaaS or GitLab self-managed:

1. Open your API by selecting **APIs** in the sidebar, and then selecting an API and a version. *Each API version can be linked to one CI project*.
1. Select the **Test** tab.
1. Under **Connect to CI/CD Builds**, select **GitLab SaaS** or **GitLab self-managed**.
1. You'll be prompted to authorize Postman to access your GitLab account. After you grant access, you can close the browser tab and return to Postman.

For GitLab SaaS, do the following:

1. Enter a **Nickname** to help you recognize the integration later. Postman pre-fills a nickname in the format `GitLab-{API_NAME}-{API_VERSION_NAME}`, and you can edit it if you want.
1. Select the GitLab **Workspace** with your API repository.
1. Select the **CI project** used for your API.
1. Select **Connect**.

<img alt="Connect to GitLab CI/CD" src="https://assets.postman.com/postman-docs/gitlab-saas-connect-ci-cd-v9-19.jpg" width="571px" />

For GitLab self-managed, do the following:

1. Enter the URL of your GitLab self-managed domain.
1. Enter your GitLab personal access token. When [creating your token](https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html), make sure to select the **api**, **read_api**, **read_user**, and **write_repository** scopes.
1. Select **Authenticate**.

<img alt="Connect to GitLab CI/CD" src="https://assets.postman.com/postman-docs/gitlab-hosted-connect-ci-cd-v9-19.jpg" width="571px" />

## Viewing build status

After you set up a GitLab integration, information for build jobs is available in Postman. For each build you can view the branch, start time, and status (successful or failed). You can also view the results of collection runs that are [configured in your pipeline using Newman](#viewing-collection-run-details).

To view build jobs, open an API version and select the **Test** tab. The most recent jobs are listed under **CI/CD Builds**.

Select **View All Builds** to view the full list of build jobs. From here you can take the following actions:

* Use the dropdown lists to filter jobs by branch or build status.
* To view a build in GitLab, select the build name.
* To start a new build, select **Run Build**. Select or enter the name of the branch to use, and then select **Run Build**. To cancel a running build, select **Cancel** next to the build.
* To get the latest build status information, select <img alt="Refresh icon" src="https://assets.postman.com/postman-docs/icon-refresh-v9-5.jpg#icon" width="14px"> **Refresh**.
* To edit or delete the integration, select the more actions icon <img alt="More actions icon" src="https://assets.postman.com/postman-docs/icon-more-actions-v9.jpg#icon" width="16px">.
* To view more details for a build, use the arrows to expand a build and expand **Build Steps**. For each build step you can view the name, duration, and status.

<img alt="View all GitLab builds" src="https://assets.postman.com/postman-docs/gitlab-builds-v9-19.jpg">

## Viewing collection run details

Using Newman, you can run Postman collections with your API tests as part of a GitLab pipeline. The Postman cloud reporter can send details about the collection runs back to Postman.

To view details for collections that were run as part of a build, first [configure Newman for GitLab](#configuring-newman-for-gitlab-cicd) and then [start a new build](#viewing-build-status). After the build is complete, use the arrows to expand a build and expand **Collection Runs**. Then expand a collection to view details about a collection run.

<img alt="View collection runs" src="https://assets.postman.com/postman-docs/gitlab-collection-runs-v9-19.jpg">

> Select **View Report** to view a collection run report in the Postman **History**. Learn more about using the [Collection Runner](/docs/running-collections/intro-to-collection-runs/).

## Configuring Newman for GitLab CI/CD

With the help of Newman and the Postman API, you can run API tests created in Postman as part of your GitLab pipeline. First generate the Newman configuration code in Postman. Then add the configuration code to the `.gitlab-ci.yml` file in your GitLab repository.

Each time the pipeline runs, Newman runs the collections that contain your tests. You can view the results of your tests in Postman. You an also configure the [Postman cloud reporter](https://www.npmjs.com/package/newman-reporter-postman-cloud) to send detailed collection run information back to Postman.

> Before you begin, make sure you’ve already [set up an integration](#configuring-a-gitlab-cicd-integration) between your API version and GitLab CI/CD.

To generate configuration code for Newman:

1. Open your API version and select the **Test** tab.
1. Under **CI/CD Builds**, select **View All Builds**.
1. Select **Configure Newman**.
1. Select a **Collection** to run during pipeline builds. To be available in the dropdown list, you must first [add the collection as a test suite](/docs/designing-and-developing-your-api/testing-an-api/#adding-tests) to your API. You can also select an **Environment** to use.

    > If needed, select **+ Add More** to select other collections to run.

1. (Optional) Select the checkbox to use the Postman cloud reporter to send collection run information back to Postman. You can view the collection run details in the Postman **History** and on the API version **Test** tab.
1. Select **Copy** to copy the Newman configuration, and then select **Finish**.

   <img alt="Generate Newman configuration" src="https://assets.postman.com/postman-docs/gitlab-ci-generate-newman-v9-16.jpg" width="447px" />

To add the Newman configuration to your GitLab pipeline:

1. Edit the `.gitlab-ci.yml` file at the root of your Gitlab repository.
1. Add the Newman configuration you copied from Postman to the `.gitlab-ci.yml` file:
    * Replace all instances of `$POSTMAN_API_KEY` with a valid [Postman API Key](/docs/developer/intro-api/#generating-a-postman-api-key).
1. Commit and push the changes to your remote repository. This will automatically start a build in GitLab.
1. To view the test results in Postman, open your API and select the **Test** tab. Learn more about [Viewing collection run details](#viewing-collection-run-details).

### Example .gitlab-ci.yml file

```yaml
stages:
  - build

build_code_job:
    stage: build
    script:
        - npm i -g newman
        - npm i -g newman-reporter-postman-cloud
        - newman run "https://api.getpostman.com/collections/4946945-3673316a-9a35-4b0d-a148-3566b490798d?apikey=$POSTMAN_API_KEY"  --environment "https://api.getpostman.com/environments/16724969-8e6c6119-ed57-4665-b4f9-f648c5637484?apikey=$POSTMAN_API_KEY" -r postman-cloud --reporter-apiKey "$POSTMAN_API_KEY" --reporter-workspaceId "34f3a42c-18a7-4ad6-83fb-2c05767d63a7" --reporter-integrationIdentifier 47056-${CI_PIPELINE_ID}
```
