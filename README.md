# Picnic OSS Maven Parent

[![Build Status][travisci-badge]][travisci-builds]
[![Maven Central][maven-central-badge]][maven-central-browse]
[![SonarCloud Quality Gate][sonarcloud-badge-quality-gate]][sonarcloud-dashboard]
[![SonarCloud Bugs][sonarcloud-badge-bugs]][sonarcloud-measure-reliability]
[![SonarCloud Vulnerabilities][sonarcloud-badge-vulnerabilities]][sonarcloud-measure-security]
[![SonarCloud Maintainability][sonarcloud-badge-maintainability]][sonarcloud-measure-maintainability]

The Maven parent POM for open-source Java projects by Picnic. This POM closely
resembles the one used for Picnic-internal projects.

## How to Use

Artifacts are hosted on [Maven's Central Repository][maven-central-search]. To
use the POM in your own project, declare it as a parent in your `pom.xml` file:

```xml
<parent>
    <groupId>tech.picnic</groupId>
    <artifactId>oss-parent</artifactId>
    <version>0.0.1</version>
</parent>
```

## Contributing

Contributions are welcome! Feel free to file an [issue][new-issue] or open a
[pull request][new-pr]. Note that any contribution must either be compatible
with Picnic projects that use the parent, or allow for those projects to be
made compatible.

When submitting changes, please make every effort to follow existing
conventions and style in order to keep the configuration as readable as
possible.

[maven-central-badge]: https://img.shields.io/maven-central/v/tech.picnic/oss-parent.svg
[maven-central-browse]: https://repo1.maven.org/maven2/tech/picnic/oss-parent
[maven-central-search]: https://search.maven.org
[new-issue]: https://github.com/PicnicSupermarket/oss-parent/issues/new
[new-pr]: https://github.com/PicnicSupermarket/oss-parent/compare
[sonarcloud-badge-bugs]: https://sonarcloud.io/api/project_badges/measure?project=tech.picnic%3Aoss-parent&metric=bugs
[sonarcloud-badge-maintainability]: https://sonarcloud.io/api/project_badges/measure?project=tech.picnic%3Aoss-parent&metric=sqale_rating
[sonarcloud-badge-quality-gate]: https://sonarcloud.io/api/project_badges/measure?project=tech.picnic%3Aoss-parent&metric=alert_status
[sonarcloud-badge-vulnerabilities]: https://sonarcloud.io/api/project_badges/measure?project=tech.picnic%3Aoss-parent&metric=vulnerabilities
[sonarcloud-dashboard]: https://sonarcloud.io/dashboard?id=tech.picnic%3Aoss-parent
[sonarcloud-measure-reliability]: https://sonarcloud.io/component_measures?id=tech.picnic%3Aoss-parent&metric=Reliability
[sonarcloud-measure-security]: https://sonarcloud.io/component_measures?id=tech.picnic%3Aoss-parent&metric=Security
[sonarcloud-measure-maintainability]: https://sonarcloud.io/component_measures?id=tech.picnic%3Aoss-parent&metric=Maintainability
[travisci-badge]: https://travis-ci.org/PicnicSupermarket/oss-parent.svg?branch=master
[travisci-builds]: https://travis-ci.org/PicnicSupermarket/oss-parent
