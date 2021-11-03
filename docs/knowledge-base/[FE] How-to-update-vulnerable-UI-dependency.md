# How to update vulnerable UI dependency

There are many cases one can encounter when trying to update a
dependency that contains a known vulnerability to the version where it’s
fixed. This page describes approaches engineers can take to deal with
them.

> *Ongoing effort: given the numerous cases how dependencies can be specified in npm packages, this page will be populated with more content as engineers encounter those cases.*

## Direct dependency

Trivial case if the dependency is specified in our own
<a href="https://github.com/stackrox/rox/tree/master/ui" class="external-link">package.json</a>
file, update it directly, for example using
`yarn upgrade --latest {dependency_name}`.

## Indirect dependency

### Dependent package accepting update that contains the fix

Conditions:

-   Vulnerable dependency is used by some 3d-party package

-   Fix for dependency exists (usually as a patch version)

-   3d-party dependent package in its `package.json` file specifies that
    it accepts updates for this dependency (e.g. `dompurify 2.0.6` has a vulnerability and `2.0.7` has it fixed,
    `redoc` that depends on `dompurify` specified the dependency this
    way `"dompurify": "^2.0.3"` meaning it accepts any patch releases
    after `2.0.3`).

In this case you need to let `yarn` to pull the latest acceptable
version by:

1.  Find the `yarn.lock` entry for the vulnerable dependency and
    directly change the `yarn.lock` by deleting the whole entry

2.  Run `yarn install`, which will pull the latest version (in the
    scenario with `dompurify` it was `2.0.7`) and it’ll update
    `yarn.lock` to add the entry that the dependency is resolved to this
    version

3.  Commit updated `yarn.lock` file.

## Related articles

- How to run vuln audit and analysis for UI dependencies
