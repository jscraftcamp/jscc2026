# Bettervibe.org guide

https://bettervibe.org/guide/

This is the guide from the workshop that was attended. The focus in the session was highlighting how to enable the agent to produce better quality output. This is achieved by giving the agent pre-commit/pre-push hooks that cover:

* linting
* formatting
* testing

Also, that you should use the tools that benefit mostly the agent, not necessarily yourself. For example using Rust because of its extensive compiler, meaning if it compiles, it is already of a pretty good quality.  Or using tools like GraphQL or API Schemas, so the agent has more information about the various interfaces. 

## Skills

BetterVibe also provides skills in this repo: https://github.com/bettervibe-org/skills

Most noteworthy is the "bootstrap project" skill, that lets you set up a new project with principles laid out in the guide.

A companion skill to that is a vibe-check-agentic-engineering skill, that rates your repository on how well it implements those principles with a full HTML report.

# Pi Coding agent

https://pi.dev/

The session went briefly over this coding agent. It is a minimal agent harness with almost no functionality. But that is intended, because Pi contains all the tools to extend itself, so you as the user can choose what feature should go into that harness.

There is also a package registry of already created extensions.

One good analogy that came up was:

Pi is like the Arch Linux of agents, and the page registry is the AUR. I use Pi btw.

# Offline-first git based Wiki

There is a 7 year attempt at an offline first markdown wiki software that was based on PouchDB and syncing to CouchDB, but which was abandoned.

Shown here was a second attempt, with the difference that the markdown files are saved in a git repository and stored in [OPFS](https://developer.mozilla.org/en-US/docs/Web/API/File_System_API/Origin_private_file_system). So all you need to share the data between different devices is a simple git repository which can be hosted anywhere.

Unfortunately Cross-Origin Resource Sharing security policies in the Browser only allow pushing to that remote Git repository if it is on the same domain or sends the correct headers. So unless you have control over that you still need to rely on a server-side proxy that does the requests for you.

This new implementation was created entirely by using the principles in the BetterVibe guide with a focus on (almost) no dependencies.

That is why [Deno](https://deno.com/) was chosen as the runtime/build tool, because it comes with TypeScript, testing, linter and formatter out of the box. The only dependency used is [isomorphic-git](https://isomorphic-git.org/), which is a pure JavaScript implementation of git that can run in the browser.

The project will be made public under [pouch.wiki](https://pouch.wiki/) which, as of the time of the session, still features the old implementation.