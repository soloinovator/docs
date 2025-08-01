---
title: About secret scanning
intro: '{% data variables.product.github %} scans repositories for known types of secrets, to prevent fraudulent use of secrets that were committed accidentally.'
product: |
  {% data reusables.gated-features.secret-scanning %}{% ifversion secret-risk-assessment %}

  {% data variables.secret-scanning.secret-risk-assessment-cta-product %}{% endif %}
redirect_from:
  - /github/administering-a-repository/about-token-scanning
  - /articles/about-token-scanning
  - /articles/about-token-scanning-for-private-repositories
  - /github/administering-a-repository/about-secret-scanning
  - /code-security/secret-security/about-secret-scanning
  - /code-security/secret-scanning/about-secret-scanning
versions:
  fpt: '*'
  ghes: '*'
  ghec: '*'
type: overview
topics:
  - Secret scanning
  - Secret Protection
shortTitle: Secret scanning
---

## About {% data variables.product.prodname_secret_scanning %}

{% data variables.product.prodname_secret_scanning_caps %} is a security feature that helps detect and prevent the accidental inclusion of sensitive information such as API keys, passwords, tokens, and other secrets in your repository. When enabled, {% data variables.product.prodname_secret_scanning %} scans commits in repositories for known types of secrets and alerts repository administrators upon detection.

{% data variables.product.prodname_secret_scanning_caps %} scans your entire Git history on all branches present in your {% data variables.product.prodname_dotcom %} repository for secrets, even if the repository is archived. {% data variables.product.prodname_dotcom %} will also periodically run a full Git history scan for new secret types in existing content in {% ifversion fpt or ghec %}public repositories{% else %}repositories with {% data variables.product.prodname_GH_secret_protection %} enabled{% endif %} where {% data variables.product.prodname_secret_scanning %} is enabled when new supported secret types are added.

{% data reusables.secret-scanning.what-is-scanned %}

{% ifversion ghas-products %}{% ifversion secret-risk-assessment %}
> [!TIP]
> Regardless of the enablement status of {% data variables.product.prodname_AS %} features, organizations on {% data variables.product.prodname_team %} and {% data variables.product.prodname_enterprise %} can run a free report to scan the code in the organization for leaked secrets.
>
> To generate a report, open {% data reusables.security-overview.navigate-to-risk-assessment %}.{% endif %}{% else %}{% endif %}

When a supported secret is leaked, {% data variables.product.github %} generates a {% data variables.product.prodname_secret_scanning %} alert. Alerts are reported on the **Security** tab of repositories on {% data variables.product.github %}, where you can view, evaluate, and resolve them. For more information, see [AUTOTITLE](/code-security/secret-scanning/managing-alerts-from-secret-scanning).

{% ifversion fpt or ghec %}Service providers can partner with {% data variables.product.company_short %} to provide their secret formats for scanning. We automatically run {% data variables.product.prodname_secret_scanning %} for partner patterns on all public repositories and public npm packages.{% data reusables.secret-scanning.partner-program-link %}

Any strings that match patterns that were provided by secret scanning partners are reported directly to the relevant partner, and aren't displayed on {% data variables.product.prodname_dotcom %}. For more information about partner patterns, see [AUTOTITLE](/code-security/secret-scanning/managing-alerts-from-secret-scanning/about-alerts).{% endif %}

For information about the secrets and service providers supported by {% data variables.product.prodname_secret_scanning %}, see [AUTOTITLE](/code-security/secret-scanning/introduction/supported-secret-scanning-patterns#supported-secrets).

You can use the REST API to monitor results from {% data variables.product.prodname_secret_scanning %} across your repositories or organization. For more information about API endpoints, see [AUTOTITLE](/rest/secret-scanning).

You can also use security overview to see an organization-level view of which repositories have enabled {% data variables.product.prodname_secret_scanning %} and the alerts found. For more information, see [AUTOTITLE](/code-security/security-overview/about-security-overview).

{% data reusables.secret-scanning.audit-secret-scanning-events %}

## How {% data variables.product.prodname_secret_scanning %} works

Below is a typical workflow that explains how {% data variables.product.prodname_secret_scanning %} works:

* **Detection:** {% data variables.product.prodname_secret_scanning_caps %} automatically scans your repository's contents for sensitive data, such as API keys, passwords, tokens, and other secrets. It looks for patterns and heuristics that match known types of secrets.

* **Alerts:** When a potential secret is detected, {% data variables.product.prodname_dotcom %} generates an alert and notifies the relevant repository administrators and users. This notification includes details about the detected secret, such as its location in the repository. For more information about alert types and alert details, see [AUTOTITLE](/code-security/secret-scanning/managing-alerts-from-secret-scanning/about-alerts).

* **Review:** When a secret is detected, you'll need to review the alert details provided.

* **Remediation:** You then need to take appropriate action to remediate the exposure. This should always include rotating the affected credential to ensure it is no longer usable.  It may also include removing the secret from the repository's history (using tools like `git-filter-repo`; see [AUTOTITLE](/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository) for more details) though this will likely involve a heavy cost in time and effort, and is usually unnecessary if the credentials have been revoked.

* **Monitoring:** It's good practice to regularly audit and monitor your repositories to ensure no other secrets are exposed.

{% ifversion fpt or ghec %}

* **Integration with partners:** {% data variables.product.prodname_dotcom %} works with various service providers to validate secrets. When a partner secret is detected, {% data variables.product.prodname_dotcom %} notifies the provider so they can take appropriate action, such as revoking the credential. For more information about the partnership program, see [AUTOTITLE](/code-security/secret-scanning/secret-scanning-partnership-program/secret-scanning-partner-program).

{% endif %}

## About the benefits of {% data variables.product.prodname_secret_scanning %}

* **Enhanced security:** {% data variables.product.prodname_secret_scanning_caps %} scans your repositories for sensitive information like API keys, passwords, tokens, and other secrets. By detecting these early, you can mitigate potential security risks before they are exploited by malicious actors.

* **Automated detection:** The feature automatically scans your codebase, including commits, issues, and pull requests, ensuring continuous protection without requiring manual intervention. This automation helps in maintaining security even as your repository evolves.

* **Real-time alerts:** When a secret is detected, {% data variables.product.prodname_secret_scanning %} provides real-time alerts to repository administrators and contributors. This immediate feedback allows for swift remediation actions.

{% ifversion fpt or ghec %}

* **Integration with service providers:** {% data variables.product.prodname_dotcom %} partners with various service providers to validate detected secrets. When a secret is identified, {% data variables.product.prodname_dotcom %} notifies the corresponding service provider to take appropriate actions, such as revoking the exposed credential. For more information, see [AUTOTITLE](/code-security/secret-scanning/secret-scanning-partnership-program/secret-scanning-partner-program).

{% endif %}

* **Custom pattern support:** Organizations can define custom patterns to detect proprietary or unique types of secrets that may not be covered by default patterns. This flexibility allows for tailored security measures specific to your environment.

* **Ability to detect non-provider patterns:** You can expand the detection to include non-provider patterns such as connection strings, authentication headers, and private keys, for your repository or organization.

## Customizing {% data variables.product.prodname_secret_scanning %}

Once {% data variables.product.prodname_secret_scanning %} is enabled, you can customize it further:

### Detection of non-provider patterns

Scan for and detect secrets that are not specific to a service provider, such as private keys and generic API keys. For more information, see [AUTOTITLE](/code-security/secret-scanning/using-advanced-secret-scanning-and-push-protection-features/non-provider-patterns/enabling-secret-scanning-for-non-provider-patterns).

### Performing validity checks

Validity checks help you prioritize alerts by telling you which secrets are `active` or `inactive`. For more information, see{% ifversion secret-scanning-validity-check-partner-patterns %} [AUTOTITLE](/code-security/secret-scanning/enabling-secret-scanning-features/enabling-validity-checks-for-your-repository) and{% endif %} [AUTOTITLE](/code-security/secret-scanning/managing-alerts-from-secret-scanning/evaluating-alerts#checking-a-secrets-validity).

### Defining custom patterns

Define your own patterns for secrets used by your organization that {% data variables.product.prodname_secret_scanning %} can scan for and detect. For more information, see [AUTOTITLE](/code-security/secret-scanning/using-advanced-secret-scanning-and-push-protection-features/custom-patterns/defining-custom-patterns-for-secret-scanning).

{% ifversion secret-scanning-ai-generic-secret-detection %}

### {% data variables.secret-scanning.copilot-secret-scanning %}

* **{% data variables.secret-scanning.generic-secret-detection-caps %}:** Leverage {% data variables.product.prodname_secret_scanning %}'s AI capabilities to detect unstructured secrets, such as passwords, in your repository. For more information, see [AUTOTITLE](/code-security/secret-scanning/copilot-secret-scanning/responsible-ai-generic-secrets).{% ifversion secret-scanning-custom-pattern-ai-generated %}
* **{% data variables.secret-scanning.custom-pattern-regular-expression-generator-caps %}:** Leverage {% data variables.product.prodname_secret_scanning %}'s AI capabilities to generate regular expressions that will capture all your custom patterns. For more information, see [AUTOTITLE](/code-security/secret-scanning/copilot-secret-scanning/responsible-ai-regex-generator).{% endif %}

{% endif %}

## Further reading

* [AUTOTITLE](/code-security/secret-scanning/enabling-secret-scanning-features/enabling-secret-scanning-for-your-repository)
* [AUTOTITLE](/code-security/secret-scanning/introduction/about-push-protection)
* [AUTOTITLE](/code-security/secret-scanning/working-with-secret-scanning-and-push-protection)
* [AUTOTITLE](/code-security/getting-started/best-practices-for-preventing-data-leaks-in-your-organization)
* [AUTOTITLE](/code-security/getting-started/securing-your-repository)
* [AUTOTITLE](/authentication/keeping-your-account-and-data-secure)
