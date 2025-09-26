# upgraded 폴더 구조 설명

`upgraded` 폴더는 레거시(`legacy`) 프로젝트의 전체 파일과 폴더를 복사하여 최신화 작업을 진행할 공간입니다. 각 구성 요소는 다음과 같습니다:

- `MANIFEST.in`: 패키징 시 포함할 파일 목록을 지정하는 설정 파일
- `README.rst`: 프로젝트 설명서
- `distribute-0.6.10.tar.gz`, `distribute_setup.py`: 레거시 Python 패키징 관련 파일
- `setup.py`: 프로젝트 설치 및 배포를 위한 스크립트
- `docs/`: Sphinx 기반의 프로젝트 문서 디렉터리
    - `source/`: 문서 원본(rst, conf.py 등)
    - `build/`: 빌드된 문서(html, doctree 등)
- `guachi/`: 주요 Python 소스 코드 디렉터리
    - `__init__.py`, `config.py`, `database.py`: 핵심 모듈
    - `tests/`: 단위 및 통합 테스트 코드
- `guachi.egg-info/`: 패키징 정보 디렉터리

이 폴더는 레거시 코드를 최신 Python 버전에 맞게 업그레이드하고, 테스트 및 문서화 작업을 진행하는 공간입니다.