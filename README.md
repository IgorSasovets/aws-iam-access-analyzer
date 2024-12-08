# IAM Access Analyzer Custom Policy Checks Demo Repository

## Overview

This repository demonstrates the use of AWS IAM Access Analyzer Custom Policy Checks, a powerful feature that helps you proactively identify critical permissions and potential risks in IAM policies. These checks leverage automated reasoning to provide higher levels of security assurance and can be integrated into CI/CD pipelines or used locally for policy evaluation.

**Key Features Demonstrated**:
* **_Check No New Access (check-no-new-access)_**: Verifies that an updated policy does not introduce new permissions compared to the previous version.
* **_Check Access Not Granted (check-access-not-granted)_**: Ensures specified access is not allowed by a policy, aligning with security requirements.
* **_Check No Public Access (check-no-public-access)_**: Ensures resource policies do not grant public access to specified resource types.

## Repository Structure

```
├── workflows/
│   ├── check-no-new-access.yml         # GitHub Action workflow for `check-no-new-access`
│   ├── check-access-not-granted.yml    # GitHub Action workflow for `check-access-not-granted`
│   ├── check-no-public-access.yml      # GitHub Action workflow for `check-no-public-access`
├── policies/                           # Sample policies used for the demo purposes│
├── README.md                           # This README file
```

## Usage Instructions

**Prerequisites**

* AWS CLI installed and configured with appropriate permissions.
* GitHub Actions enabled for your repository (if using workflows).
* IAM Access Analyzer enabled in your AWS account.

**Running Custom Policy Checks Locally**

* Check No New Access
```
aws accessanalyzer policy-check check-no-new-access \
    --policy-type IDENTITY_POLICY \
    --current-policy file://example_new_policy.json \
    --previous-policy file://example_old_policy.json
```

* Check Access Not Granted

```
aws accessanalyzer policy-check check-access-not-granted \
    --policy-type IDENTITY_POLICY \
    --policy file://example_new_policy.json \
    --access-constraint "action=sts:AssumeRole,principal=*"
```

* Check No Public Access

```
aws accessanalyzer policy-check check-no-public-access \
    --policy-type RESOURCE_POLICY \
    --policy file://public_resource_policy.json \
    --resource-type S3_BUCKET
```

**Using GitHub Actions**

Step 1: Create a repository secret for AWS credentials
```
    Go to your repository’s Settings > Secrets and variables > Actions.
    Add secrets for AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY, and AWS_REGION.
```

Step 2: Trigger workflows
```
    Push a new policy or update an existing policy in the policies/ directory.
    Run the corresponding workflow manually, evaluating the policy with the specified check.
```

## How It Works

Custom Policy Checks Overview
1. check-no-new-access: Compares two policy versions and ensures no new permissions are added.
2. check-access-not-granted: Checks a policy against defined access constraints.
3. check-no-public-access: Validates that a policy does not unintentionally allow public access.

## Examples

1. Checking for New Access in a Policy
```
aws accessanalyzer policy-check check-no-new-access \
    --policy-type IDENTITY_POLICY \
    --current-policy file://example_new_policy.json \
    --previous-policy file://example_old_policy.json
```
2. Ensuring Access is Not Granted
```
aws accessanalyzer policy-check check-access-not-granted \
    --policy-type IDENTITY_POLICY \
    --policy file://example_new_policy.json \
    --access-constraint "action=sts:AssumeRole,principal=*"
```
3. Validating No Public Access
```
aws accessanalyzer policy-check check-no-public-access \
    --policy-type RESOURCE_POLICY \
    --policy file://public_resource_policy.json \
    --resource-type S3_BUCKET
```

## Contributing

Contributions are welcome! Please submit a pull request with clear descriptions and testing details.