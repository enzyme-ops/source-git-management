# Source Git Management
A guide for managing code via Git.

## Code Management
**GitFlow** is the preferred method for managing code, branches, releases etc. A full walkthrough can be found here [Gitflow Workflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

## Versioning
**Semantic Versioning** MUST be used for all releases, a full walkthrough can be found here [SemVer](http://semver.org/). Is it crutial as each breaking change (major version bump) will require a new autoscaling group, ELB and CodeDeploy deployment group to retain the zero-downtime characteristics when using the setup described here [Laravel CodeDeploy Template](https://github.com/enzyme-ops/laravel-code-deploy-template).

Minor and patch releases can be deployed without fear of breaking the application for active users when SemVer is followed correctly. During a major release, a new ELB will be spun up, a new autoscaling group and CodeDeploy deployment group will be created and the application will be deployed across it. After testing, the current domains DNS records can be updated to reflect the new ELB endpoint and all active and future users will be propagated over to the new release without disruption. Once you believe all users are on the new version and DNS has been given enough time to propagate globally, the old release and services stack can be removed from AWS.

### Examples of version bumping
Let's start with this base example block of code:
```php
function doStuff($amount) {
    $total = $amount + 1.5;

    persist($total);
}
```

**Patch version bump**
```php
function doStuff($amount) {
    $total = $amount + 1.6; // 1.5 was the wrong amount.

    persist($total);
}
```

**Minor version bump**
```php
function doStuff($amount) {
    $total = $amount + 1.5;

    persist($total);
    log($amount); // Backwards compatible feature addition.
}
```

**Major version bump**
```php
// Function name change, not backwards compatible with pre-existing code.
function doOtherStuff($amount) {
    $total = $amount + 1.5;

    persist($total);
}
```
