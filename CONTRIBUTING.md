# verifirewall Contributing Guide🌴

Thank you for your interest in verifirewall. We welcome contributions of all kinds, there is no need to do code to be helpful! All of the following tasks are noble and worthy contributions that you can make without coding:

- Reporting security vulnerabilities
- Reporting a bug
- Helping a member of the community
- Notes about our documentation
- Providing feedback and feature requests

Before making any kind of contribution, read our [Code of Conduct](./CODE_OF_CONDUCT.md) to keep our community approachable and respectable.

This guide will provide an overview of the various contribution options' guidelines - from reporting or fixing a bug to suggesting an enhancement.

## Reporting security vulnerabilities

If you've found a vulnerability or a potential vulnerability in verifirewall please let us know at [security-alert@verifirewall.io](mailto:security-alert@verifirewall.io). We'll send a confirmation email to acknowledge your report within 24 hours and send an additional email when we've identified the issue positively or negatively.

An internal process will be activated upon determining the validity of a reported security vulnerability, which will end with releasing a fix and deciding on the appropriate disclosure actions. The reporter of the issue will receive updates on this process' progress.

## Reporting a bug

**Important - If the bug you wish to report regards a suspicion of a security vulnerability, please refer to the [Reporting security vulnerability](#Reporting-security-vulnerabilities) section**

To report a bug, you can either open a new issue using a relevant [issue form](https://github.com/github/docs/issues/new/choose) or, [contact us via our verifirewall open source distribution list](mailto:opensource@verifirewall.io).

Be sure to include a **title and clear description**, as much relevant information as possible, and a **code sample** or an **executable test case** demonstrating the expected behavior that is not occurring.

### Bug Report Template

When filing a bug report, please include:

- **Environment**: OS, kernel version, NGINX/Kong/Envoy/APISIX version
- **verifirewall version**: Output of `verifirewall --version` or package version
- **Steps to reproduce**: Minimal steps to trigger the issue
- **Expected behavior**: What should happen
- **Actual behavior**: What actually happens
- **Logs**: Relevant log snippets (sanitize sensitive data)
- **Configuration**: Relevant config files (remove secrets)

## Contributing a fix to a bug

Please [contact us via our verifirewall open source distribution list](mailto:opensource@verifirewall.io) before writing your code. We will want to make sure we understand the boundaries of the proposed fix, that the relevant coding style is clear for the proposed fix's location in the code, and that the suggested contribution is relevant and eligible.

Once you've received our confirmation follow the next steps:

1.  Fork the repository to your GitHub account.
2. Clone your forked repository to your local machine.
3. Add your contributions to relevant locations in the local copy of the codebase.
4. Push your changes back to your forked repository.
5. Open a pull request (PR) against the main branch of the original repository.

## Contributing code-independent enhancements

For any code-independent enhancements (such as docker-compose files, or instructions on how to compile on different OSs) please follow the next steps:

1. [suggest your change via our verifirewall open-source distribution list](mailto:opensource@verifirewall.io) to inform us about your possible contribution and wait for our confirmation.
2. Fork the repository to your GitHub account.
3. Clone your forked repository to your local machine.
4. Add your contributions to the "contrib" Folder in the local copy of the codebase.
5. Push your changes back to your forked repository.
6. Open a pull request (PR) against the main branch of the original repository.

Please note that during the PR review we might adjust the location of the contributions.

## Proposing an enhancement

Please [suggest your change via our verifirewall open-source distribution list](mailto:opensource@verifirewall.io) before writing your code. We will contact you to make sure we understand the boundaries of the proposed fix, that the relevant coding style is clear for the proposed fix's location in the code, and that the suggested contribution is relevant and eligible. There may be additional considerations that we would like to discuss with you before implementing the enhancement.

## Open Source documentation issues

To propose changes to our [documentation](https://docs.verifirewall.io/?utm_medium=web&utm_source=wix&utm_content=top_menu) you can either open a new issue using a relevant [issue form](https://github.com/github/docs/issues/new/choose) or, [contact us via our verifirewall open source distribution list](mailto:opensource@verifirewall.io).

## Development Setup

### Prerequisites

- C++17 compatible compiler (GCC 8+, Clang 7+)
- CMake 3.16+
- Boost, OpenSSL, PCRE2, libxml2, GTest, GMock, cURL, Redis, Hiredis, MaxmindDB, yq

### Building

```bash
git clone https://github.com/verifirewall/verifirewall.git
cd verifirewall
cmake -DCMAKE_INSTALL_PREFIX=build_out .
make install
make package
```

### Testing

```bash
# Run unit tests
cd build_out
ctest --output-on-failure

# Run static analysis
cppcheck --enable=all --std=c++17 --suppress=missingIncludeSystem .
```

### Coding Standards

- Follow the existing code style in the repository
- Use 4 spaces for indentation (no tabs)
- Line length: 120 characters maximum
- Include license header in new files
- Document public APIs with Doxygen comments
- Write tests for new functionality

### Pull Request Guidelines

1. **Title**: Clear, descriptive title (e.g., "Fix: Handle null pointer in HTTP parser")
2. **Description**: Explain the change, why it's needed, and how it was tested
3. **Scope**: Keep PRs focused - one logical change per PR
4. **Tests**: Add/update tests for your changes
5. **Documentation**: Update docs if behavior changes
6. **CI**: Ensure all checks pass

## Final thanks

We value all efforts to read, suggest changes, and/or contribute to our open-source files. Thank you for your time and efforts.

The verifirewall Team