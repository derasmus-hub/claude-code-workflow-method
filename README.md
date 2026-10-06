# My Claude Code Workflow Method

This is the way I have learned to work with Claude Code after using coding agents regularly on real projects.

It is not a framework or a product. It is simply a working method I developed because I kept seeing the same problems when coding sessions became longer and more complicated.

A few people on LinkedIn asked me how I work with Claude Code, so I decided to document it.

## Why this exists

When I first started using Claude Code, I noticed that the beginning of a session could be very productive, but the results became less predictable as the work became larger.

Vague instructions produced vague results.

Long conversations created context drift.

A simple request like "fix this" could turn into several rounds of changes without a clear definition of what finished actually meant.

I started breaking work into smaller steps with a specific result that could be verified before moving forward.

That worked much better for me.

The method below developed gradually from using coding agents while building larger software projects.

I am not claiming this is the best way to use Claude Code. It is simply the method that has worked well for me.

## The three patterns

### 1. Numbered self contained prompts

For larger tasks, I prefer to break the work into a numbered sequence instead of having one long conversation.

Each step should contain enough context for Claude Code to understand the task and should end with something that can be verified.

A simple example might look like this:

```text
1. Read CLAUDE.md and the current state of src/intake/.

Summarize what currently exists.

Do not change any code yet.

Verify by returning a short summary of what you found.


2. Add POST /intake/submit.

Accept name, email and level.

Validate the input with Pydantic.

Return 201 when a record is created.

Verify by calling the endpoint with a sample request and showing the response.


3. Add tests for the endpoint.

Test the normal case, a missing field and an invalid email.

Verify by running pytest and showing the result.
```

Each step has a purpose.

Each step has an observable result.

If something goes wrong, I know where it went wrong instead of trying to diagnose an entire session at once.

### 2. Verification

Verification is probably the most important part of the method.

I do not want the coding agent to tell me that something looks correct.

I want it to show me something that demonstrates the result.

Examples include:

```text
Run pytest and show the result.

Call the endpoint and show the response.

Run git diff --stat and show what changed.

List the files in the directory and confirm the expected file exists.

Run the application and show whether it starts successfully.
```

A weak verification instruction would be:

```text
Make sure it works.
```

That does not define what success means.

A better instruction defines something observable that both the agent and I can check.

This has become especially important for me when working on larger projects where one incorrect assumption can affect several later steps.

### 3. Bounded task delegation

Once the goal and verification criteria are clear, I am comfortable giving a coding agent more room to work independently.

For example:

```text
Here are the acceptance criteria for this feature:

[clear and testable requirements]

Read the current implementation.

Identify anything that does not meet the requirements.

Make the necessary changes.

Run the relevant tests.

If a test fails, investigate the failure and try again.

Report what you changed and show the final test results.
```

The important part for me is that the task has boundaries.

The agent knows what it is responsible for.

The success criteria are already defined.

There is still a point where I review the result before moving forward.

I have found this much more reliable than giving an agent a broad instruction and hoping it makes the same assumptions I would.

## What this method is not

### It is not a replacement for thinking

I still have to decide what I want to build, what the requirements are and what finished should look like.

The coding agent can help with implementation, but it still needs a clear target.

### It is not fully autonomous

I do not assume that something is correct simply because the agent says it is finished.

I review important changes and use tests and other observable results to verify the work.

### It is not always the fastest approach

For a very small change, I may simply make the change or give the agent one instruction.

The structured approach becomes more useful when a task has several steps or when mistakes early in the process could create problems later.

### It is not how I use AI for everything

Exploration and debugging are different.

Sometimes I want to discuss a problem, investigate possibilities or let the agent explore.

The structured method is mainly how I approach implementation work when I already have a reasonable idea of what needs to be built.

## Things I had to stop doing

### Letting the conversation drift

Something like this can become a problem quickly:

```text
Now add this.

Also change that.

Actually refactor this too.

And while you are there, fix this other thing.
```

Eventually the original task becomes unclear.

I prefer to stop, redefine the task and start again with clear scope.

### Using vague verification

"Make sure it works" does not tell me much.

A test result, API response or observable application behavior does.

### Skipping the initial review

On an existing project, I usually want the coding agent to inspect the relevant code before changing anything.

If it misunderstands how the current system works, everything it builds afterward can be based on the wrong assumption.

### Accepting completion without evidence

When an agent says something is implemented, I still want to see the result.

For me, completion and verification are two separate things.

## Where this method came from

This method developed gradually from working with coding agents on larger projects.

As the projects became more complicated, I found that vague instructions became less useful and small misunderstandings became more expensive.

Breaking work into clear steps, defining what success means and verifying the result before continuing made the process much more reliable.

Over time that became the way I prefer to work with coding agents.

I continue changing the method as the tools improve and as I learn what works better.

## Feedback

I am still refining this.

If you use Claude Code or other coding agents differently and have found an approach that works well, I would be interested in hearing about it.

If you try this method and find something that does not work, I would be interested in that too.

Built by [Duan Erasmus](https://www.linkedin.com/in/duan-erasmus)
