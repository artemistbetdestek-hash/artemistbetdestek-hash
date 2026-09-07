## Hi there 👋

<!--
**artemistbetdestek-hash/artemistbetdestek-hash** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
-->Skip to main content
GitHub Docs
Home
GitHub Actions
Reference
Workflows and actions
Contexts
Contexts reference
Find information about contexts available in GitHub Actions workflows, including available properties, access methods, and usage examples.

In this article
Available contexts
Context name	Type	Description
github	object	Information about the workflow run. For more information, see github context.
env	object	Contains variables set in a workflow, job, or step. For more information, see env context.
vars	object	Contains variables set at the repository, organization, or environment levels. For more information, see vars context.
job	object	Information about the currently running job. For more information, see job context.
jobs	object	For reusable workflows only, contains outputs of jobs from the reusable workflow. For more information, see jobs context.
steps	object	Information about the steps that have been run in the current job. For more information, see steps context.
runner	object	Information about the runner that is running the current job. For more information, see runner context.
secrets	object	Contains the names and values of secrets that are available to a workflow run. For more information, see secrets context.
strategy	object	Information about the matrix execution strategy for the current job. For more information, see strategy context.
matrix	object	Contains the matrix properties defined in the workflow that apply to the current job. For more information, see matrix context.
needs	object	Contains the outputs of all jobs that are defined as a dependency of the current job. For more information, see needs context.
inputs	object	Contains the inputs of a reusable or manually triggered workflow. For more information, see inputs context.
As part of an expression, you can access context information using one of two syntaxes.

Index syntax: github['sha']
Property dereference syntax: github.sha
In order to use property dereference syntax, the property name must start with a letter or _ and contain only alphanumeric characters, -, or _.

If you attempt to dereference a nonexistent property, it will evaluate to an empty string.

Determining when to use contexts
GitHub Actions includes a collection of variables called contexts and a similar collection of variables called default variables. These variables are intended for use at different points in the workflow:

Default environment variables: These environment variables exist only on the runner that is executing your job. For more information, see Variables reference.
Contexts: You can use most contexts at any point in your workflow, including when default variables would be unavailable. For example, you can use contexts with expressions to perform initial processing before the job is routed to a runner for execution; this allows you to use a context with the conditional if keyword to determine whether a step should run. Once the job is running, you can also retrieve context variables from the runner that is executing the job, such as runner.os. For details of where you can use various contexts within a workflow, see Context availability.
The following example demonstrates how these different types of variables can be used together in a job:

YAML
name: CI
on: push
jobs:
  prod-check:
    if: ${{ github.ref == 'refs/heads/main' }}
    runs-on: ubuntu-latest
    steps:
      - run: echo "Deploying to production server on branch $GITHUB_REF"
In this example, the if statement checks the github.ref context to determine the current branch name; if the name is refs/heads/main, then the subsequent steps are executed. The if check is processed by GitHub Actions, and the job is only sent to the runner if the result is true. Once the job is sent to the runner, the step is executed and refers to the $GITHUB_REF variable from the runner.

Context availability
Different contexts are available throughout a workflow run. For example, the secrets context may only be used at certain places within a job.

In addition, some functions may only be used in certain places. For example, the hashFiles function is not available everywhere.

The following table lists the restrictions on where each context and special function can be used within a workflow. The listed contexts are only available for the given workflow key, and may not be used anywhere else. Unless listed below, a function can be used anywhere.

Workflow key	Context	Special functions
run-name	github, inputs, vars	None
concurrency	github, inputs, vars	None
env	github, secrets, inputs, vars	None
jobs.<job_id>.concurrency	github, needs, strategy, matrix, inputs, vars	None
jobs.<job_id>.container	github, needs, strategy, matrix, vars, inputs	None
jobs.<job_id>.container.credentials	github, needs, strategy, matrix, env, vars, secrets, inputs	None
jobs.<job_id>.container.env.<env_id>	github, needs, strategy, matrix, job, runner, env, vars, secrets, inputs	None
jobs.<job_id>.container.image	github, needs, strategy, matrix, vars, inputs	None
jobs.<job_id>.continue-on-error	github, needs, strategy, vars, matrix, inputs	None
jobs.<job_id>.defaults.run	github, needs, strategy, matrix, env, vars, inputs	None
jobs.<job_id>.env	github, needs, strategy, matrix, vars, secrets, inputs	None
jobs.<job_id>.environment	github, needs, strategy, matrix, vars, inputs	None
jobs.<job_id>.environment.url	github, needs, strategy, matrix, job, runner, env, vars, steps, inputs	None
jobs.<job_id>.if	github, needs, vars, inputs	always, cancelled, success, failure
jobs.<job_id>.name	github, needs, strategy, matrix, vars, inputs	None
jobs.<job_id>.outputs.<output_id>	github, needs, strategy, matrix, job, runner, env, vars, secrets, steps, inputs	None
jobs.<job_id>.runs-on	github, needs, strategy, matrix, vars, inputs	None
jobs.<job_id>.secrets.<secrets_id>	github, needs, strategy, matrix, secrets, inputs, vars	None
jobs.<job_id>.services	github, needs, strategy, matrix, vars, inputs	None
jobs.<job_id>.services.<service_id>.credentials	github, needs, strategy, matrix, env, vars, secrets, inputs	None
jobs.<job_id>.services.<service_id>.env.<env_id>	github, needs, strategy, matrix, job, runner, env, vars, secrets, inputs	None
jobs.<job_id>.steps.continue-on-error	github, needs, strategy, matrix, job, runner, env, vars, secrets, steps, inputs	hashFiles
jobs.<job_id>.steps.env	github, needs, strategy, matrix, job, runner, env, vars, secrets, steps, inputs	hashFiles
jobs.<job_id>.steps.if	github, needs, strategy, matrix, job, runner, env, vars, steps, inputs	always, cancelled, success, failure, hashFiles
jobs.<job_id>.steps.name	github, needs, strategy, matrix, job, runner, env, vars, secrets, steps, inputs	hashFiles
jobs.<job_id>.steps.run	github, needs, strategy, matrix, job, runner, env, vars, secrets, steps, inputs	hashFiles
jobs.<job_id>.steps.timeout-minutes	github, needs, strategy, matrix, job, runner, env, vars, secrets, steps, inputs	hashFiles
jobs.<job_id>.steps.with	github, needs, strategy, matrix, job, runner, env, vars, secrets, steps, inputs	hashFiles
jobs.<job_id>.steps.working-directory	github, needs, strategy, matrix, job, runner, env, vars, secrets, steps, inputs	hashFiles
jobs.<job_id>.strategy	github, needs, vars, inputs	None
jobs.<job_id>.timeout-minutes	github, needs, strategy, matrix, vars, inputs	None
jobs.<job_id>.with.<with_id>	github, needs, strategy, matrix, inputs, vars	None
on.workflow_call.inputs.<inputs_id>.default	github, inputs, vars	None
on.workflow_call.outputs.<output_id>.value	github, jobs, vars, inputs	None
Example: printing context information to the log
You can print the contents of contexts to the log for debugging. The toJSON function is required to pretty-print JSON objects to the log.

Warning

When using the whole github context, be mindful that it includes sensitive information such as github.token. GitHub masks secrets when they are printed to the console, but you should be cautious when exporting or printing the context.

YAML
name: Context testing
on: push

jobs:
  dump_contexts_to_log:
    runs-on: ubuntu-latest
    steps:
      - name: Dump GitHub context
        env:
          GITHUB_CONTEXT: ${{ toJson(github) }}
        run: echo "$GITHUB_CONTEXT"
      - name: Dump job context
        env:
          JOB_CONTEXT: ${{ toJson(job) }}
        run: echo "$JOB_CONTEXT"
      - name: Dump steps context
        env:
          STEPS_CONTEXT: ${{ toJson(steps) }}
        run: echo "$STEPS_CONTEXT"
      - name: Dump runner context
        env:
          RUNNER_CONTEXT: ${{ toJson(runner) }}
        run: echo "$RUNNER_CONTEXT"
      - name: Dump strategy context
        env:
          STRATEGY_CONTEXT: ${{ toJson(strategy) }}
        run: echo "$STRATEGY_CONTEXT"
      - name: Dump matrix context
        env:
          MATRIX_CONTEXT: ${{ toJson(matrix) }}
        run: echo "$MATRIX_CONTEXT"
github context
The github context contains information about the workflow run and the event that triggered the run. You can read most of the github context data in environment variables. For more information about environment variables, see Store information in variables.

Warning

When using the whole github context, be mindful that it includes sensitive information such as github.token. GitHub masks secrets when they are printed to the console, but you should be cautious when exporting or printing the context. When creating workflows and actions, you should always consider whether your code might execute untrusted input from possible attackers. Certain contexts should be treated as untrusted input, as an attacker could insert their own malicious content. For more information, see Secure use reference.

Property name	Type	Description
github	object	The top-level context available during any job or step in a workflow. This object contains all the properties listed below.
github.action	string	The name of the action currently running, or the id of a step. GitHub removes special characters, and uses the name __run when the current step runs a script without an id. If you use the same action more than once in the same job, the name will include a suffix with the sequence number with underscore before it. For example, the first script you run will have the name __run, and the second script will be named __run_2. Similarly, the second invocation of actions/checkout will be actionscheckout2.
github.action_path	string	The path where an action is located. This property is only supported in composite actions. You can use this path to access files located in the same repository as the action, for example by changing directories to the path (using the corresponding environment variable): cd "$GITHUB_ACTION_PATH" . For more information on environment variables, see Secure use reference.
github.action_ref	string	For a step executing an action, this is the ref of the action being executed. For example, v2.

Do not use in the run keyword. To make this context work with composite actions, reference it within the env context of the composite action.
github.action_repository	string	For a step executing an action, this is the owner and repository name of the action. For example, actions/checkout.

Do not use in the run keyword. To make this context work with composite actions, reference it within the env context of the composite action.
github.action_status	string	For a composite action, the current result of the composite action.
github.actor	string	The username of the user that triggered the initial workflow run. If the workflow run is a re-run, this value may differ from github.triggering_actor. Any workflow re-runs will use the privileges of github.actor, even if the actor initiating the re-run (github.triggering_actor) has different privileges.
github.actor_id	string	The account ID of the person or app that triggered the initial workflow run. For example, 1234567. Note that this is different from the actor username.
github.api_url	string	The URL of the GitHub REST API.
github.artifacts	string	Path on the runner to the file that identifies workflow artifacts for the current step. Write one declaration per line to identify files or OCI digest references as workflow artifacts. For more information, see Workflow commands for GitHub Actions.
github.artifacts_list	string	Path on the runner to a read-only file containing the aggregated workflow artifact metadata for the current job as JSON. For more information, see Workflow commands for GitHub Actions.
github.base_ref	string	The base_ref or target branch of the pull request in a workflow run. This property is only available when the event that triggers a workflow run is either pull_request or pull_request_target.
github.env	string	Path on the runner to the file that sets environment variables from workflow commands. This file is unique to the current step and is a different file for each step in a job. For more information, see Workflow commands for GitHub Actions.
github.event	object	The full event webhook payload. You can access individual properties of the event using this context. This object is identical to the webhook payload of the event that triggered the workflow run, and is different for each event. The webhooks for each GitHub Actions event is linked in Events that trigger workflows. For example, for a workflow run triggered by the push event, this object contains the contents of the push webhook payload.
github.event_name	string	The name of the event that triggered the workflow run.
github.event_path	string	The path to the file on the runner that contains the full event webhook payload.
github.graphql_url	string	The URL of the GitHub GraphQL API.
github.head_ref	string	The head_ref or source branch of the pull request in a workflow run. This property is only available when the event that triggers a workflow run is either pull_request or pull_request_target.
github.job	string	The job_id of the current job.
Note: This context property is set by the Actions runner, and is only available within the execution steps of a job. Otherwise, the value of this property will be null.
github.path	string	Path on the runner to the file that sets system PATH variables from workflow commands. This file is unique to the current step and is a different file for each step in a job. For more information, see Workflow commands for GitHub Actions.
github.ref	string	The fully-formed ref of the branch or tag that triggered the workflow run. For workflows triggered by push, this is the branch or tag ref that was pushed. For workflows triggered by pull_request that were not merged, this is the pull request merge branch. If the pull request was merged, this is the branch it was merged into. For workflows triggered by release, this is the release tag created. For other triggers, this is the branch or tag ref that triggered the workflow run. This is only set if a branch or tag is available for the event type. The ref given is fully-formed, meaning that for branches the format is refs/heads/<branch_name>. For pull request events except pull_request_target that were not merged, it is refs/pull/<pr_number>/merge. pull_request_target events have the ref from the base branch. For tags it is refs/tags/<tag_name>. For example, refs/heads/feature-branch-1. For more information about pull request merge branches, see Pull requests.
github.ref_name	string	The short ref name of the branch or tag that triggered the workflow run. This value matches the branch or tag name shown on GitHub. For example, feature-branch-1.

For pull requests that were not merged, the format is <pr_number>/merge.
github.ref_protected	boolean	true if branch protections or rulesets are configured for the ref that triggered the workflow run.
github.ref_type	string	The type of ref that triggered the workflow run. Valid values are branch or tag.
github.repository	string	The owner and repository name. For example, octocat/Hello-World.
github.repository_id	string	The ID of the repository. For example, 123456789. Note that this is different from the repository name.
github.repository_owner	string	The repository owner's username. For example, octocat.
github.repository_owner_id	string	The repository owner's account ID. For example, 1234567. Note that this is different from the owner's name.
github.repositoryUrl	string	The Git URL to the repository. For example, git://github.com/octocat/hello-world.git.
github.retention_days	string	The number of days that workflow run logs and artifacts are kept.
github.run_id	string	A unique number for each workflow run within a repository. This number does not change if you re-run the workflow run.
github.run_number	string	A unique number for each run of a particular workflow in a repository. This number begins at 1 for the workflow's first run, and increments with each new run. This number does not change if you re-run the workflow run.
github.run_attempt	string	A unique number for each attempt of a particular workflow run in a repository. This number begins at 1 for the workflow run's first attempt, and increments with each re-run.
github.secret_source	string	The source of a secret used in a workflow. Possible values are None, Actions, Codespaces, or Dependabot.
github.server_url	string	The URL of the GitHub server. For example: https://github.com.
github.sha	string	The commit SHA that triggered the workflow. The value of this commit SHA depends on the event that triggered the workflow. For more information, see Events that trigger workflows. For example, ffac537e6cbbf934b08745a378932722df287a53.
github.token	string	A token to authenticate on behalf of the GitHub App installed on your repository. This is functionally equivalent to the GITHUB_TOKEN secret. For more information, see Use GITHUB_TOKEN for authentication in workflows.
Note: This context property is set by the Actions runner, and is only available within the execution steps of a job. Otherwise, the value of this property will be null.
github.triggering_actor	string	The username of the user that initiated the workflow run. If the workflow run is a re-run, this value may differ from github.actor. Any workflow re-runs will use the privileges of github.actor, even if the actor initiating the re-run (github.triggering_actor) has different privileges.
github.workflow	string	The name of the workflow. If the workflow file doesn't specify a name, the value of this property is the full path of the workflow file in the repository.
github.workflow_ref	string	The ref path to the workflow. For example, octocat/hello-world/.github/workflows/my-workflow.yml@refs/heads/my_branch.
github.workflow_sha	string	The commit SHA for the workflow file.
github.workspace	string	The default working directory on the runner for steps, and the default location of your repository when using the checkout action.
Example contents of the github context
The following example context is from a workflow run triggered by the push event. The event object in this example has been truncated because it is identical to the contents of the push webhook payload.

Note

This context is an example only. The contents of a context depends on the workflow that you are running. Contexts, objects, and properties will vary significantly under different workflow run conditions.

{
  "token": "***",
  "job": "dump_contexts_to_log",
  "ref": "refs/heads/my_branch",
  "sha": "c27d339ee6075c1f744c5d4b200f7901aad2c369",
  "repository": "octocat/hello-world",
  "repository_owner": "octocat",
  "repositoryUrl": "git://github.com/octocat/hello-world.git",
  "run_id": "1536140711",
  "run_number": "314",
  "retention_days": "90",
  "run_attempt": "1",
  "actor": "octocat",
  "workflow": "Context testing",
  "head_ref": "",
  "base_ref": "",
  "event_name": "push",
  "event": {
    ...
  },
  "server_url": "https://github.com",
  "api_url": "https://api.github.com",
  "graphql_url": "https://api.github.com/graphql",
  "ref_name": "my_branch",
  "ref_protected": false,
  "ref_type": "branch",
  "secret_source": "Actions",
  "wor
