# Security Policy

## Trust model

PHP_CodeSniffer standards and configuration are not sandboxed.

Rulesets for PHP_CodeSniffer will instruct PHP_CodeSniffer to load and execute PHP code supplied by PHP_CodeSniffer itself and the CodeSniffer related dependencies of the analyzed project.  
Rulesets may also contain instructions to load and execute PHP code supplied by the analyzed project, including custom sniffs and explicitly autoloaded files.  
Repository-local PHPCS configuration should therefore be treated as executable project code.

Running PHP_CodeSniffer against an untrusted or attacker-modifiable checkout must be treated in the same way as running that checkout's tests, build scripts, or other development tooling.

CI environments processing untrusted contributions are responsible for appropriate isolation, including restricting access to credentials, secrets, privileged services, and sensitive network resources.

## Supported Versions

For all repositories in the PHPCSStandards organisation, the latest patch version of the current major is supported for security updates.

Security patches will be backported to a previous major branch for up to a year after the last (non-security) release for that major.

## Reporting a Vulnerability

All packages in the PHPCSStandards organisation are developer tools and should generally not be used in a production (web accessible) environment.

Having said that, responsible disclosure of security issues is highly appreciated.

**Please do not report or discuss security vulnerabilities through public GitHub issues, discussions, or pull requests.**

Issues can be reported privately to the maintainers by opening a Security vulnerability report in the appropriate repository.

### Preferences

* Please provide detailed reports with reproducible steps and a clearly defined impact.
* Include the version number of the vulnerable package in your report.
* Fixes are most welcome.
    A private PR can be created from the security report to work on and discuss the patch.
