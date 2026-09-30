# cicd-student-demo
# CI/CD Implementation Using GitHub Actions

## 1. Project Objective

The objective of this project is to implement Continuous Integration (CI) and Continuous Deployment (CD) using GitHub Actions.

The project automatically tests a Python application and deploys a website to GitHub Pages only when all tests pass.

## 2. Technologies Used

* Python
* pytest
* GitHub Actions
* GitHub Pages
* HTML
* YAML
* Git and GitHub

## 3. Project Structure

```text
cicd-student-demo/
├── .github/
│   └── workflows/
│       └── cicd.yml
├── index.html
├── student_result.py
├── test_student_result.py
└── README.md
```

## 4. Python Application

File: `student_result.py`

```python
def check_result(mark):
    if mark >= 40:
        return "Pass"
    else:
        return "Fail"
```

This function checks whether a student passes or fails.

* Marks of 40 or above: Pass
* Marks below 40: Fail

## 5. Automated Testing

File: `test_student_result.py`

```python
from student_result import check_result

def test_pass():
    assert check_result(65) == "Pass"

def test_fail():
    assert check_result(30) == "Fail"

def test_boundary():
    assert check_result(40) == "Pass"
```

These tests verify the application using pytest.

## 6. Continuous Integration (CI)

Continuous Integration automatically runs tests whenever code is pushed to the `main` branch.

### CI workflow steps

1. Download the source code.
2. Set up Python.
3. Install pytest.
4. Run automated tests.
5. Report whether the tests pass or fail.

## 7. GitHub Actions Workflow

File: `.github/workflows/cicd.yml`

```yaml
name: Complete CI/CD Pipeline

on:
  push:
    branches:
      - main
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

jobs:
  test:
    name: CI - Test Application
    runs-on: ubuntu-24.04

    steps:
      - name: Download source code
        uses: actions/checkout@v7

      - name: Set up Python
        uses: actions/setup-python@v7
        with:
          python-version: "3.13"

      - name: Install pytest
        run: pip install pytest

      - name: Run automated tests
        run: pytest -v

  deploy:
    name: CD - Deploy Website
    needs: test
    runs-on: ubuntu-24.04

    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}

    steps:
      - name: Download source code
        uses: actions/checkout@v7

      - name: Configure GitHub Pages
        uses: actions/configure-pages@v5

      - name: Upload website
        uses: actions/upload-pages-artifact@v4
        with:
          path: .

      - name: Deploy website
        id: deployment
        uses: actions/deploy-pages@v4
```

## 8. Important Workflow Configuration

The workflow contains two jobs:

* **test:** Runs the Python application tests.
* **deploy:** Deploys the website to GitHub Pages.

The following line connects the two jobs:

```yaml
needs: test
```

It ensures that the deployment job runs only after the test job succeeds.

If the tests fail, deployment is skipped.

## 9. Continuous Deployment (CD)

Continuous Deployment automatically publishes the website after the CI tests pass.

### CD workflow steps

1. Configure GitHub Pages.
2. Upload the website files.
3. Deploy the website.
4. Make the website available online.

## 10. Deployment Webpage

File: `index.html`

```html
<!DOCTYPE html>
<html>
<head>
    <title>Student CI/CD Demo</title>
</head>
<body>
    <h1>Student CI/CD Demonstration</h1>
    <h2>Deployment Successful</h2>

    <p>
        This webpage was deployed automatically
        using GitHub Actions.
    </p>

    <p>Application Status: Running</p>
</body>
</html>
```

## 11. GitHub Pages Configuration

To enable deployment:

1. Open the repository on GitHub.
2. Click **Settings**.
3. Select **Pages**.
4. Under the build and deployment settings, choose **GitHub Actions** as the source.
5. Save the settings if prompted.

## 12. Successful Pipeline

When code is pushed to the `main` branch:

1. GitHub Actions starts the workflow.
2. Python is configured.
3. pytest runs the automated tests.
4. If all tests pass, the deployment job starts.
5. GitHub Pages publishes the website.

The workflow can also be started manually using `workflow_dispatch`.

## 13. CI Failure Demonstration

To demonstrate the failure of automated tests, temporarily change the condition in `student_result.py`:

```python
if mark >= 80:
    return "Pass"
```

The tests for marks 65 and 40 will fail because the expected result is `"Pass"`.

### Expected result

* The CI test job fails.
* The deployment job is skipped.
* The website is not redeployed by that failed workflow run.

## 14. Fixing the Application

Restore the original condition:

```python
if mark >= 40:
    return "Pass"
```

Commit and push the changes to GitHub.

GitHub Actions will run the tests again. When all tests pass, the deployment job can run.

## 15. Continuous Integration vs Continuous Deployment

| Continuous Integration (CI) | Continuous Deployment (CD)             |
| --------------------------- | -------------------------------------- |
| Tests application code      | Publishes the website                  |
| Detects errors              | Makes the application available online |
| Uses pytest                 | Uses GitHub Pages deployment actions   |
| Runs first                  | Runs after CI succeeds                 |

## 16. Complete CI/CD Architecture

```text
Developer
    |
    v
Push Code to GitHub
    |
    v
GitHub Actions Workflow
    |
    v
CI: Install Python and pytest
    |
    v
Run Automated Tests
    |
    v
Are Tests Passing?
    |
    +---- No ----> Stop Deployment
    |
    +---- Yes ---> CD: Upload Website
                         |
                         v
                   Deploy to GitHub Pages
                         |
                         v
                    Live Website
```

## 17. Key Learning Outcomes

* Created a Python application.
* Wrote automated tests using pytest.
* Configured GitHub Actions workflows.
* Implemented Continuous Integration.
* Implemented Continuous Deployment.
* Connected CI and CD using `needs: test`.
* Demonstrated how failed tests prevent deployment.
* Published a website using GitHub Pages.

## 18. Conclusion

This project demonstrates a complete CI/CD pipeline using GitHub Actions. The pipeline automatically tests a Python application and deploys a website only after the tests succeed. It helps reduce manual work, identify errors early, and automate website deployment.
