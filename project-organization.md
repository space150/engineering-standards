[back to table of contents](readme.md)

(*this document is part of the space150 Engineering Standards & Guidelines*)

# Project Organization

## Overview

The initial organization and convention decisions on any project can create long lasting effects, both positive and negative. At the start of any project, proper care and consideration should be given to the structure and organization. It is equally important that all engineers at space150 understand our standard organization conventions for long-lasting maintenance.

## Project Documentation

Each project must provide documentation in the form of a `readme.md` at the root of the project. In cases where projects may require more robust documentation, a docs folder with further markdown files can be created at the root.

At minimum, a project's `readme.md` documentation must contain:

- A brief description of what the project is
- URLs (all environments)
- Detailed instructions to setup a local development environment - an engineer should be able to setup their own environment without assistance by reading the instructions
- Deployment instructions - (note: per our security notes, do not store production credentials or information with the repository - [see source control security](source-control.md#security))

### **Dos and Dont's**
- Do include a `readme.md` for your documentation
- Do include a troubleshooting guide in your markdown, this is very helpful for other engineers
- Don't include lengthy comments in code trying to serve as documentation
- Don't include sensitive information like credentials or client secrets in documentation

## Framework & Library Selection

While space150 supports a wide range of project types where frameworks and decisions can vary, there is a rolling set of accepted standards projects should follow. The following is not an exhaustive list but covers the most common projects.

When choosing your own frameworks and libraries, we are looking for frameworks that are:
- Modern. They should make our code simpler and more secure out-of-the-box with little-to-no configuration.
- Active. They are actively maintained by contributors. If a library hasn't been touched in over a year, start looking for a replacement.
- Popular. Popular codebases will have a large community to leverage. Nobody likes debugging an obscure project's edge-case problem.
- Stable. Good dependencies will have unit tests and CI monitors to make sure that public releases are stable.

### spaceBase
[spaceBase](https://github.com/space150/spaceBase) is space150's official Sass starter-kit that we use across all of our web projects. It sets up your Sass architecture and normalizes your CSS and native HTML elements. It also provides the structural groundwork for your application and can be tailored for individual projects.

### Next.js
[Next.js](https://www.npmjs.com/package/next) is our preferred framework for developing Node.js-backed websites and APIs. You should use it unless you have a specific reason not to; an example being if the client specifically requests that we build an `Angular` app, or that there is a specific limitation of Next.js that is a hard requirement on your project.

### Underscore.js
[Underscore.js](https://www.npmjs.com/package/underscore) provides a set of utility functions that help fill in gaps of the JavaScript language. Most notably, it has Array utilities for `map`, `reduce`, `find`, and `sort` that can be used across all browsers. 

It is getting a little less necessary these days as modern browsers get better with standardization, but you will spot this library in many of our projects.

### Axios
[Axios](https://www.npmjs.com/package/axios) is an HTTP library for both browser and server JavaScript applications. It is mature, it is tested across all platforms & browsers, it is well-documented, and it is both extremely popular and active.

### Logging
Consider using a logging framework to track application behaviour over time. Our clients frequently want to know what happened to a users experience at a certain date and time, and determining this without logging this becomes nearly impossible. Back-end services should have at least some level of persistent logging applied to them tracking incoming requests and outgoing responses.

If you're using Node.js, we've had success with the [winston](https://www.npmjs.com/package/winston) logging framework paired with [winston-daily-rotate-file](https://www.npmjs.com/package/winston-daily-rotate-file).

- Use `info`, `warn`, and `error` levels appropriately. Most of your logs will likely be `info`; these provide any point-in-time information that you feel could be useful to know when debugging. Use `warn` to provide an indication that something went wrong, but that the application will continue processing. `error` logs mark a situation that the application could not handle and must abort.
- Consider using daily rolling log files to allow for quick `find out what happened at 2pm on x/y/zzzz date` lookups.
- Be mindful about the security implications of logging data -- do not log Personally Identifiable Information, API keys, credit card information, or anything else that must remain private.