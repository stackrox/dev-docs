# How to run vuln audit and analysis for UI dependencies

StackRox UI uses a ton of npm dependencies that are being bundled
together during the build phase and included as a series of minified JS
bundles. Some of these dependencies might have vulnerabilities
discovered and reported as CVEs.

## Why do the audit and address found vulnerabilities?

-   Vulnerabilities may result in data exposure and a whole list of bad
    things, we have / had customers who expose StackRox UI to public
    (relying on authentication only), so the risk is real.

-   Having known critical vulnerabilities in our software for a long
    period of time might violate some of the customer contracts (looking
    at you, government).

-   Every year StackRox goes through the penetration testing with report
    going to our customers, so eventually they might be discovered and
    we might be asked to provide an explanation and report when this
    vulnerability was introduced etc. (extra work for us)

-   Customers do vuln scanning of our images, and can become concerned
    if critical vulns are found. While it’s very unlikely
    that any vuln scanner is sophisticated enough to analyze minified
    bundled JS code, it’s possible theoretically.

## Doing an audit

### Required Setup

-   Installed
    <a href="https://nodejs.org/en/" class="external-link">Node.js</a>,
    e.g. you can install LTS version using
    <a href="https://github.com/nvm-sh/nvm" class="external-link">nvm</a>.

-   Installed
    <a href="https://yarnpkg.com/en/" class="external-link">yarn</a>

### Running audit

1.  Go to
    <a href="https://github.com/stackrox/rox/tree/master/ui" class="external-link">rox/ui</a>
    directory of checked out StackRox Platform source code

2.  Run `yarn audit --groups dependencies` (`--groups` flag tells `yarn`
    to look only into prod dependencies and ignore dev and optional
    dependencies)

### Analyzing audit report

Most of the output will be self-explanatory. Typically any HIGH or
CRITICAL vuln justifies a priority bump for the effort to update this
dependency in the next release or even a patch release.

## Related articles

- How to update vulnerable UI dependency
