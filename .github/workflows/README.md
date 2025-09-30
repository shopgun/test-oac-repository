 # GitHub Workflows for the test-oac-repository

This directory contains GitHub Actions workflows that automate various tasks in this repository.

## Fork PR Handler Workflow

The `fork-pr-handler.yml` workflow is designed to handle Pull Requests from forked repositories and enable automated scanning of external contributions while maintaining security.

### How it Works

When you create a Pull Request from a forked repository, the following automated process happens:

1. **Initial Scan**: 
   - The workflow checks your PR and leaves an initial comment acknowledging receipt
   - It verifies if your PR branch name doesn't conflict with existing branches in the main repository
   - It checks if your branch is in sync with the lates t `main` branch

   **Sample Comment:**
   ```
   👋 Thanks for your contribution! We are performing a scan of your PR...
   ```

2. **Branch Name Check**:
   - If your branch name already exists in the main repository, the workflow will fail and ask you to use a different branch name

   **Sample Comment (if branch name conflict exists):**
   ```
   ⚠️ A branch with the name `your-branch-name` already exists in the upstream repository. 
   This would cause conflicts when mirroring your PR. Please create a new branch with a different name and submit your PR from that branch instead.
   ```

3. **Sync Check**:
   - If your branch is not synced with the latest `main` branch, the workflow will fail and ask you to update

   **Sample Comment (if branch is not synced):**
   ```
   ⚠️ Your branch is not synced with the latest changes from our main branch. Please update your fork with the latest changes before we can run our workflows.
   ```

4. **Branch Mirroring**:
   - If all checks pass, your PR branch is mirrored into the main repository
   - A corresponding "mirror PR" is created in the main repository with the prefix `[PREVIEW COPY]`

   **Sample Comment (when PR is successfully mirrored):**
   ```
   ✅ Your PR has been mirrored to our repository as PR #42 (https://github.com/newrelic/test-oac-repository/pull/42).
   **Commit:** abc1234567890abcdef1234567890abcdef12345 ([abc1234](https://github.com/your-username/test-oac-repository/commit/abc1234567890abcdef1234567890abcdef12345)).
   Our workflows will run in the mirrored PR linked above.
   ```

5. **Automated Testing**:
   - Tests are triggered on the mirror PR (not on your original PR)
   - Test results are visible in the mirror PR

6. **Updates**:
   - When you push new commits to your PR, they are automatically mirrored
   - The mirror PR is updated with your changes
   - Each update triggers a new test run

   **Sample Comment (when updates are mirrored):**
   ```
   ✅ Your updates have been mirrored to our repository in PR #42 (https://github.com/newrelic/test-oac-repository/pull/42).
   **Commit:** def1234567890abcdef1234567890abcdef12345 ([def1234](https://github.com/your-username/test-oac-repository/commit/def1234567890abcdef1234567890abcdef12345))
   ```

7. **In the mirror PR, your updates will be acknowledged:**
   
   **Sample Comment (in mirror PR):**
   ```
   ♻️ This PR has been updated with the latest changes from the fork PR #15.
   **New Commit:** def1234567890abcdef1234567890abcdef12345 ([def1234](https://github.com/your-username/test-oac-repository/commit/def1234567890abcdef1234567890abcdef12345))
   ```

### Guidelines for Contributors

#### Creating a PR from a Fork

1. **Fork the Repository**:
   - Click the "Fork" button at the top right of the repository page
   - Clone your forked repository to your local machine

2. **Create a Unique Branch Name**:
   - Use a descriptive and unique branch name that doesn't exist in the main repository
   - Example: `fix/your-username/issue-description` or `feature/your-username/feature-name`

3. **Keep Your Branch Updated**:
   - Before submitting your PR, ensure your branch is up-to-date with the latest `main`:
   ```bash
   git remote add upstream https://github.com/newrelic/test-oac-repository.git
   git fetch upstream
   git checkout main
   git pull upstream main
   git checkout your-branch-name
   git rebase main
   ```

4. **Submit Your PR**:
   - Push your branch to your fork and create a Pull Request against the `main` branch of the original repository

#### What to Expect After Submitting

1. **Initial Comment**:
   - The workflow will leave a comment acknowledging your PR
   - It will perform initial validation checks

2. **Possible Validation Issues**:
   - If your branch name conflicts with an existing branch, you'll be asked to create a new branch with a different name
   - If your branch is not synced with the latest `main`, you'll be asked to update your branch

3. **Mirror PR Creation**:
   - If all checks pass, you'll see a comment indicating your PR was mirrored to the main repository
   - The comment will include a link to the mirror PR and information about the commit that was mirrored

4. **Test Execution**:
   - Tests will run on the mirror PR in the main repository
   - You can view test results in the mirror PR

5. **Updates**:
   - When you push updates to your branch, they will be automatically mirrored
   - You'll receive a comment confirming the update was mirrored

