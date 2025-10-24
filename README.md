# 🤖 AI-Powered Python API Test Automation Framework

![Python](https://img.shields.io/badge/python-3.8%2B-blue)
![Pytest](https://img.shields.io/badge/pytest-7.4.3-green)
![Allure](https://img.shields.io/badge/allure-2.13.2-orange)

A comprehensive API test automation framework that leverages AI for intelligent test validation, enhanced reporting with Allure, and extensible architecture.

## ✨ Key Features

- 🧠 **AI-Powered Validation**: Uses OpenAI GPT models for semantic API response validation
- 📊 **Allure Reporting**: Beautiful, interactive test reports with detailed insights
- 🔄 **Modular Architecture**: Easy to extend and maintain
- 🎯 **Pytest-based**: Industry-standard testing framework
- 📝 **Comprehensive Logging**: Detailed execution logs for debugging
- 🔌 **RESTful API Testing**: Built-in support for REST API testing

## 📁 Project Structure

```
ai-python-api-test-automation/
├── tests/                    # Test cases directory
│   ├── test_api_with_ai.py  # AI-powered API tests
│   └── conftest.py           # Pytest fixtures and configurations
├── utils/                    # Utility modules
│   ├── ai_validator.py       # AI validation helper
│   ├── api_client.py         # API client wrapper
│   └── logger.py             # Logging configuration
├── config/                   # Configuration files
│   ├── config.yaml           # Test configuration
│   └── .env.example          # Environment variables template
├── reports/                  # Generated test reports
│   └── allure-results/       # Allure test results
├── requirements.txt          # Python dependencies
├── pytest.ini                # Pytest configuration
└── README.md                 # This file
```

## 🚀 Getting Started

### Prerequisites

- Python 3.8 or higher
- pip (Python package installer)
- Allure Command Line (for viewing reports)

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/gauresh-bane/ai-python-api-test-automation.git
cd ai-python-api-test-automation
```

2. **Create virtual environment** (recommended)
```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

3. **Install dependencies**
```bash
pip install -r requirements.txt
```

4. **Install Allure Command Line** (for report generation)
```bash
# On macOS
brew install allure

# On Windows (using Scoop)
scoop install allure

# On Linux
sudo apt-add-repository ppa:qameta/allure
sudo apt-get update
sudo apt-get install allure
```

5. **Configure environment variables**
```bash
cp config/.env.example .env
# Edit .env and add your OpenAI API key
```

## 🏃 Running Tests

### Run all tests with Allure reporting
```bash
pytest --alluredir=reports/allure-results
```

### Run specific test file
```bash
pytest tests/test_api_with_ai.py --alluredir=reports/allure-results
```

### Run tests with verbose output
```bash
pytest -v --alluredir=reports/allure-results
```

### Run tests with specific markers
```bash
pytest -m "smoke" --alluredir=reports/allure-results
```

## 📊 Viewing Allure Reports

### Generate and open Allure report
```bash
allure serve reports/allure-results
```

This command will:
1. Generate the report from test results
2. Start a local web server
3. Automatically open the report in your browser

### Generate static Allure report
```bash
allure generate reports/allure-results -o reports/allure-report --clean
allure open reports/allure-report
```

## 📸 Sample Allure Report

### Overview Dashboard
The Allure report provides a comprehensive overview of test execution:

![Allure Overview](https://user-images.githubusercontent.com/placeholder/allure-overview.png)

**Key Metrics Displayed:**
- ✅ Total tests executed
- ✅ Pass/Fail/Skip statistics
- ⏱️ Execution time
- 📈 Trends over time
- 🔍 Test categorization

### Test Details View
![Allure Test Details](https://user-images.githubusercontent.com/placeholder/allure-details.png)

**Features:**
- Detailed step-by-step execution
- Request/Response payloads
- AI validation results
- Screenshots and attachments
- Execution timeline

### Trend Analysis
![Allure Trends](https://user-images.githubusercontent.com/placeholder/allure-trends.png)

**Insights:**
- Historical test results
- Flaky test identification
- Performance metrics
- Failure patterns

## 🧪 Writing New Test Cases

### Basic API Test with AI Validation

```python
import pytest
import allure
from utils.api_client import APIClient
from utils.ai_validator import AIValidator

@allure.feature('User Management')
@allure.story('Get User Details')
@allure.severity(allure.severity_level.CRITICAL)
@pytest.mark.smoke
def test_get_user_with_ai_validation():
    """Test API endpoint with AI-powered validation"""
    
    with allure.step('Initialize API client'):
        client = APIClient(base_url="https://jsonplaceholder.typicode.com")
    
    with allure.step('Send GET request to /users/1'):
        response = client.get("/users/1")
        allure.attach(str(response.json()), name="API Response", 
                     attachment_type=allure.attachment_type.JSON)
    
    with allure.step('Validate status code'):
        assert response.status_code == 200, f"Expected 200, got {response.status_code}"
    
    with allure.step('AI-powered response validation'):
        validator = AIValidator()
        validation_result = validator.validate_response(
            response=response.json(),
            expected_fields=['id', 'name', 'email', 'username'],
            context="User API should return valid user object with all required fields"
        )
        allure.attach(validation_result['feedback'], name="AI Validation", 
                     attachment_type=allure.attachment_type.TEXT)
        assert validation_result['is_valid'], validation_result['feedback']
```

### Test with Custom Markers and Parametrization

```python
import pytest
import allure

@allure.feature('Posts Management')
@pytest.mark.parametrize('post_id,expected_user_id', [
    (1, 1),
    (2, 1),
    (3, 1),
])
def test_posts_parametrized(post_id, expected_user_id):
    """Test multiple posts with parametrization"""
    # Test implementation
    pass
```

## 🎯 AI Validation Capabilities

The framework uses AI to:

1. **Semantic Validation**: Understanding the meaning and context of API responses
2. **Schema Verification**: Intelligent checking of data structures
3. **Data Quality Assessment**: Evaluating data completeness and correctness
4. **Anomaly Detection**: Identifying unusual patterns in responses
5. **Smart Assertions**: Context-aware validation beyond simple equality checks

### Example AI Validation

```python
from utils.ai_validator import AIValidator

validator = AIValidator()

# Validate that response makes sense contextually
result = validator.validate_response(
    response=api_response,
    expected_fields=['userId', 'id', 'title', 'body'],
    context="Blog post should have meaningful title and content"
)

if result['is_valid']:
    print("✅ AI Validation Passed:", result['feedback'])
else:
    print("❌ AI Validation Failed:", result['feedback'])
```

## 🔧 Configuration

### pytest.ini
```ini
[pytest]
addopts = -v --tb=short --alluredir=reports/allure-results
markers =
    smoke: Quick smoke tests
    regression: Full regression suite
    api: API related tests
    ai: Tests using AI validation
testpaths = tests
python_files = test_*.py
python_classes = Test*
python_functions = test_*
```

### Environment Variables (.env)
```bash
OPENAI_API_KEY=your_openai_api_key_here
BASE_API_URL=https://jsonplaceholder.typicode.com
LOG_LEVEL=INFO
```

## 📚 Adding New Features

### 1. Add New Test Module

1. Create new file in `tests/` directory: `test_new_feature.py`
2. Import required modules
3. Use Allure decorators for better reporting
4. Implement test functions

### 2. Extend AI Validator

1. Open `utils/ai_validator.py`
2. Add new validation methods
3. Customize AI prompts for specific use cases

### 3. Add Custom Fixtures

1. Edit `tests/conftest.py`
2. Add reusable fixtures for common test setups

```python
import pytest

@pytest.fixture
def api_client():
    from utils.api_client import APIClient
    return APIClient(base_url="https://api.example.com")

@pytest.fixture
def ai_validator():
    from utils.ai_validator import AIValidator
    return AIValidator()
```

## 🏗️ Extending the Framework

### Add New API Endpoints

1. Define endpoint methods in `utils/api_client.py`
2. Create corresponding test file
3. Add Allure annotations

### Integrate with CI/CD

```yaml
# .github/workflows/tests.yml
name: API Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: '3.9'
      - name: Install dependencies
        run: |
          pip install -r requirements.txt
      - name: Run tests
        env:
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
        run: |
          pytest --alluredir=reports/allure-results
      - name: Generate Allure Report
        uses: simple-elf/allure-report-action@master
        if: always()
        with:
          allure_results: reports/allure-results
```

## 📈 Best Practices

1. **Use Allure Annotations**: Always add `@allure.feature`, `@allure.story`, and `@allure.severity`
2. **Add Steps**: Use `with allure.step()` for better test readability
3. **Attach Evidence**: Attach requests, responses, and AI feedback to reports
4. **Meaningful Test Names**: Use descriptive test function names
5. **Independent Tests**: Each test should be independent and idempotent
6. **Environment Variables**: Never hardcode sensitive data
7. **AI Context**: Provide clear context to AI validator for better results

## 🐛 Troubleshooting

### Allure Report Not Generating
```bash
# Ensure allure-pytest is installed
pip install allure-pytest

# Check Allure CLI installation
allure --version
```

### AI Validation Errors
```bash
# Verify OpenAI API key
echo $OPENAI_API_KEY

# Check API quota and limits
```

### Test Failures
```bash
# Run with verbose output
pytest -vv

# Run specific test
pytest tests/test_api_with_ai.py::test_function_name -v
```

## 📝 Reporting Issues

If you encounter any issues:
1. Check existing issues on GitHub
2. Provide detailed error messages
3. Include test execution logs
4. Share Allure report screenshots

## 🤝 Contributing

Contributions are welcome! Please:
1. Fork the repository
2. Create a feature branch
3. Add tests for new features
4. Ensure all tests pass
5. Submit a pull request

## 📄 License

MIT License - feel free to use this framework for your projects!

## 🙏 Acknowledgments

- OpenAI for GPT API
- Allure Framework for beautiful reports
- Pytest community for the amazing testing framework

## 📧 Contact

For questions or suggestions, please open an issue on GitHub.

---

**Happy Testing! 🚀**
