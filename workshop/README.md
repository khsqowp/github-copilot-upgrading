# GitHub Copilot로 레거시 프로젝트 업그레이드하기

이 워크숍에서는 레거시(구식) Python 애플리케이션을 최신 Python 버전으로 업그레이드하는 과정을 GitHub Copilot의 도움을 받아 단계별로 진행합니다.

> [!NOTE]
> 이 저장소는 **GitHub Copilot**의 다양한 기능(예: **Copilot Chat**, **인라인 채팅**)을 소개하기 위한 것입니다. 아래 단계별 가이드에는 필요한 작업의 일반적인 설명이 포함되어 있으며, Copilot Chat 또는 인라인 채팅을 통해 필요한 명령을 생성할 수 있습니다.
>
> 각 단계(해당되는 경우)에는 Copilot의 제안이 올바른 명령과 일치하는지 검증할 수 있는 `Cheatsheet`도 포함되어 있습니다.
>
> 💡 다양한 프롬프트를 시도해 보며 Copilot의 제안 정확도가 어떻게 달라지는지 확인하세요. 예를 들어 인라인 채팅을 사용할 때, 추가 프롬프트로 응답을 세밀하게 조정할 수 있습니다.

## 워크숍 특징

이 워크숍에서는 Python 2.5 버전을 사용하던 레거시 애플리케이션을 다룹니다. 주요 특징은 다음과 같습니다:

1. 모든 의존성은 미리 설치되어 있지만, 레거시 애플리케이션은 구식 Python 버전을 사용합니다.
1. 애플리케이션은 Sqlite3과 오래된 Python 문법을 사용하므로 이를 최신 문법으로 업데이트할 수 있습니다.
1. 레거시 애플리케이션에는 기능 테스트에 활용할 수 있는 문서가 포함되어 있습니다.
1. 레거시 코드에는 단위 테스트와 기능 테스트가 모두 제공됩니다.

### 1. 에이전트로 프로젝트 탐색하기

`@workspace` 에이전트를 사용해 프로젝트의 구조와 동작을 설명받으세요.

- GitHub Copilot Chat을 열고 프롬프트 앞에 `@workspace`를 붙여 질문하세요.
- 예를 들어 "이 프로젝트는 의존성을 어떻게 설치하나요?"와 같은 질문을 할 수 있습니다.

### 2. 레거시 문제와 포팅 이슈 파악하기

Python 애플리케이션의 테스트를 실행해 보세요. 사전 설치된 `pytest`를 사용해 결과를 확인하세요. 어떤 문제가 발생하나요?

- `@workspace 이 코드를 최신 Python으로 포팅할 때 발생할 수 있는 문제는 무엇인가요?`와 같이 질문하세요.
- 가상 환경에서 의존성 설치도 시도해 보세요.

> [!NOTE]
> 왜 작동하지 않을 수 있나요? 레거시 Python은 더 이상 지원되지 않는 `distribute`를 사용하므로 의존성 설치가 실패할 수 있습니다. Copilot에게 다른 설치 방법을 제안받으세요.

### 3. 모든 코드를 업그레이드 디렉터리로 복사하기

코드를 포팅하려면 모든 코드를 `upgraded` 디렉터리로 복사해야 합니다. 이곳에서 코드를 작업하게 됩니다.

- 기능 테스트를 활용해 집중할 기능의 _슬라이스_를 결정하세요.

> [!NOTE]
> 비즈니스 로직에 따라 LLM이 명확히 알지 못하는 구성 요소가 있을 수 있으니 주의하세요.

### 4. 프로젝트 설치하기

가상 환경을 만들고 프로젝트를 설치하세요. `python setup.py develop`을 실행해 오류를 확인하세요.

- Copilot Chat에 오류를 붙여넣고 원인을 안내받으세요.
- 파일을 맥락으로 추가할 때 `#file:setup.py`를 활용하세요.
- Copilot의 제안대로 오류를 수정하고 설치를 완료하세요.

### 5. 테스트 실행하기

프로젝트에는 `pytest` 프레임워크와 CLI가 설치되어 있습니다. 단위 테스트를 실행해 오류를 확인하세요.

- 오류 출력을 Copilot에 붙여넣고 `#terminalSelection`을 활용하세요.
- Copilot의 응답을 검토하고 추가 설명을 요청하세요.
- 아직 코드를 수정하지 마세요.

> [!NOTE]
> 오류가 바로 이해되지 않거나 Python 버전이 오래되었음을 나타낼 수 있습니다. 추가 질문을 통해 오류를 더 잘 이해하고 전략을 세우세요. 프로젝트가 오래된 Python 버전임을 Copilot에 힌트로 제공할 수도 있습니다.

### 6. try/except 오류 수정하기

기능 테스트를 활용해 집중할 기능의 _슬라이스_를 결정하세요. 여기서는 try/except 오류를 수정합니다.

- Copilot에게 오래된 Python 버전을 언급하며 실제 문제 해결을 요청하세요.
- 모든 try/except 오류를 수정하고 테스트를 반복 실행해 오류가 해결되는지 확인하세요.

> [!NOTE]
> 예외 처리 외에도 다른 오류가 있을 수 있습니다. 모든 try/except를 완전히 수정한 후 다음 단계로 진행하세요.

### 7. ConfigParser 문제 해결하기

다음 슬라이스는 `ConfigParser`가 사용 불가한 문제입니다. Copilot에게 해결 전략을 요청하세요.

- "이 프로젝트는 Python 2.5로 작성되었고, Python 3.9에서 다음과 같은 오류가 발생합니다:"와 같이 질문하고 테스트 출력을 포함하세요.
- 간단한 해결책이 있을 수 있으니, 선택 후 바로 검증하세요.
- 테스트를 실행해 변경 사항이 올바른지 확인하세요.

> [!NOTE]
> 모든 테스트가 통과해야 합니다. 그렇지 않으면 Copilot에 테스트 출력을 붙여넣고 이전 단계들을 다시 검토하세요.

### 8. 테스트를 Pytest로 포팅하기

Pytest는 가장 강력한 Python 테스트 프레임워크입니다. 레거시 코드는 `unittest`를 사용하므로, 모든 테스트를 `pytest`로 포팅하세요.

- 테스트 파일에서 `@workspace` 에이전트로 포팅 방법을 질문하세요.
- 클래스 대신 함수 기반 테스트를 작성해 보세요.
- 하나의 테스트를 작성한 후 Copilot에게 다음 테스트 생성을 요청하세요.

> [!NOTE]
> `pytest`는 `unittest` 테스트도 실행할 수 있지만, 더 강력하고 유연하므로 테스트를 `pytest`로 포팅하는 것이 좋습니다. 향후 테스트 추가나 수정에도 도움이 됩니다.

### 9. 최신 패키징 방식 사용하기

레거시 코드는 `distribute`를 사용했지만, 여전히 `setup.py` 기반의 `setuptools`를 사용합니다. 최신 표준인 `pyproject.toml`로 패키징하세요.

- Copilot에게 `pyproject.toml` 파일 작성 방법을 질문하세요. `setup.py` 파일을 열어 맥락으로 활용하세요.
- 새 `pyproject.toml` 파일을 만들고 Copilot의 제안으로 내용을 채우세요.
- `pip install .`로 프로젝트를 설치하세요.

> [!NOTE]
> Python 패키징은 여전히 어려운 주제입니다. `pyproject.toml`이 표준이지만 완전히 보편화되진 않았습니다. Copilot의 제안이 항상 정확하지 않을 수 있으니 반드시 내용을 검증하고 설치를 테스트하세요.

### 10. GitHub Actions로 자동화하기

레거시 프로젝트에는 자동화가 없습니다. GitHub Actions로 테스트와 검증을 자동화하세요. 모든 PR마다 워크플로우가 실행되어 테스트와 설치를 검증하도록 합니다.

- `@workspace` 에이전트로 GitHub Actions 워크플로우 작성 방법을 질문하세요.
- 필요한 파일과 디렉터리를 만들어 워크플로우를 구성하세요.
- 변경 사항을 푸시해 GitHub Actions가 정상 동작하는지 확인하세요.

> [!NOTE]
> 자동화 구축은 현대 소프트웨어 개발의 핵심이지만, 완성까지 시간이 걸릴 수 있습니다. Copilot을 활용해 오류와 문제를 해결하며 진행하세요.
## Upgrading a Legacy Project with GitHub Copilot

Let's go through the process of taking a legacy (outdated) Python application and bring it up to the latest version of Python with the assitance of GitHub Copilot.

> [!NOTE]
> This repo is intended to give an introduction to various **GitHub Copilot** features, such as **Copilot Chat** and **inline chat**. Hence the step-by-step guides below contain the general description of what needs to be done, and Copilot Chat or inline chat can support you in generating the necessary commands.
>
> Each step (where applicable) also contains a `Cheatsheet` which can be used to validate the Copilot suggestion(s) against the correct command.
>
> 💡 Play around with different prompts and see how it affects the accuracy of the GitHub Copilot suggestions. For example, when using inline chat, you can use an additional prompt to refine the response without having to rewrite the whole prompt.

## Workshop features

In this workshop, you will be working with a legacy Python application that used Python version 2.5. Here are some features:

1. All dependencies are pre-installed for you, but not using the legacy application which uses a dated version of Python.
1. The application uses Sqlite3 and dated Python syntax that you can update
1. The legacy application has documentation that can be used for functional testing
1. Both unit and functional tests are provided as part of the legacy code. 


### 1. Explore the project using agents

Use the `@workspace` agent to explain what is going on with this project.

- Open GitHub Copilot Chat and prefix your prompt with `@workspace`
- Ask questions like how is this project installing dependencies

### 2. Determine the legacy and porting problem

Try to run the tests in the Python application. Use `pytest` which is pre-installed to get an output. What happens? 

- Ask `@workspace what are some problems that this code may have as I port it to modern Python?`
- Try installing the dependencies with a virtual environment

> [!NOTE]
> Why this might not work? Dependencies will probably fail to install as this is legacy Python using `distribute` which is no longer supported. 
> Use Copilot to suggest other ways to install the dependencies


### 3. Copy all of the code over to the ugpraded dir

In order to port the code, you will need to copy all of the code over to the
`upgraded` directory. This is where you will be working on the code.

- Use the functional tests to determine a _slice_ of functionality that you will focus on
- 
> [!NOTE]
> Why this might not provide correct explanations? Because the business logic
> may want to have some of this components which aren't obvious to the LLM


### 4. Install the project 

Create a virtual environment and install the project. Do `python setup.py develop` and try to understand what the error is. 

- Use GitHub Copilot chat to paste the error and get guidance on what is
  going on
- Ensure to add the file as context `#file:setup.py` 
- Fix the errors with the suggestions and complete the installation 


### 5. Run the tests

The project has the `pytest` framework and command-line tool installed and available. Try running the unit tests available and explore the errors.

- Use the output of the errors to ask Copilot for help. Include it in the chat with `#terminalSelection`. Make sure you have selected the output.
- Review the response from Copilot and ask for clarifications if needed
- Do *not* edit the code yet.


> [!NOTE]
> The errors might not immediately make sense or indicate that the Python version is dated. Ensure you ask further questions to get a better understanding of the errors and pick a strategy. You can also hint that an old Python version was used for the project.

### 6.Fix the try/except errors

Use the functional tests to determine a _slice_ of functionality that you will focus on. In this case you will fix the try/except errors.

- Use GitHub Copilot to help you with the actual problem to fix. Mention the old Python version.
- Fix all try/except errors and run the tests continously until these are fixed.

> [!NOTE]
> There are other errors aside from the exception ones. Make sure all try/except are clean and fixed before moving on.


### 7. Fix the ConfigParser problem

The next slice of the problem is dealing with `ConfigParser` not being available. Use GitHub Copilot to provide a strategy to choose for implementing a fix.

- Ask _"This project was built using Python 2.5 and with Python 3.9 I get the following:"_ . Include a paste of the test output
- The fix might be simple. Choose an option to implement and verify immediately.
- Run the tests to ensure that the changes are correct and that the tests pass

[!NOTE]
> All tests should now pass. If they don't, you can use the test output to ask Copilot for help and revise all the previous steps in this workshop


### 8. Port the tests to Pytest

Pytest is the most common and powerful testing framework and testing command-line tool in Python. The legacy code uses `unittest` which is not as powerful and flexible as `pytest`. Port all the tests to `pytest` and run them.

- On any test file, use the `@workspace` agent to ask how to port the tests to `pytest`
- Write a single test, and try using test functions instead of classes when 
  possible
- Once you write a test, use GitHub Copilot to suggest the next test to generate

> [!NOTE]
> Although tests can run with `pytest` because it is flexible enough to run `unittest` tests, it is better to port the tests to `pytest` as it is more powerful and flexible. This will also help you in the future when you need to add new tests or modify existing ones.

### 9. Use modern packaging

The legacy code uses `distribute` which you had to fix earlier. But it is still using `setuptools` which is now deprecated with `setup.py`. Use `pyproject.toml` to
package the project.

- Ask Copilot to help you with the `pyproject.toml` file. Use the `@workspace` agent to ask how to create a `pyproject.toml` file. Ensure you have the `setup.py` file open so that it can be used as context.
- Create a new `pyproject.toml` file and use Copilot suggestions to fill it in.
- Use the `pyproject.toml` file to install the project. You can use `pip install .` to install the project in editable mode.


> [!NOTE]
> Python packaging continues to be a challenging topic. The `pyproject.toml` file is the new standard for packaging Python projects, but it is still not widely adopted. You can use GitHub Copilot to help you with the `pyproject.toml` file, but it may not always be accurate. Make sure to verify the contents of the file and test the installation.

### 10. Automate with GitHub Actions

The legacy project didn't have any automation. Use GitHub Actions to automate testing and validation of the project. The goal is to have a workdlow that will run on every pull request and will run the tests and try to install the project.

- Ask GitHub Copilot with the `@workspace` agent how to create a GitHub Actions workflow.
- Create the necessary files and directory to create a GitHub Actions workflow.
- Push your changes when done to verify GitHub Actions is working.

> [!NOTE]
> Building automation is a key part of modern software development, but it can take a long time to get everything just right.
> Be patient while creating the automation and use GitHub Copilot to help you with errors and issues as you make progress.
