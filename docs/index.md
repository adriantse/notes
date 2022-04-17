## About <a name = "about"></a>

Personal wiki that has workflow to publish via git and mkdocs.

## Getting Started <a name = "getting_started"></a>

### Prerequisites

```
brew cask install git python
```

### Installing

```
git clone https://gitlab.com/agentc/notes.git
pip install -r requirements.txt
```

## Usage <a name="usage"></a>

```
mkdocs build
mkdocs serve
```

Visit <http://127.0.0.1:8000/> to see wiki locally

## Deployment <a name = "deployment"></a>

To deploy, gitlab has a pipeline setup defined in .gitlab-ci.yml

Make your changes, commit and push.

## Built Using <a name = "built_using"></a>

- [mkdocs](https://github.com/mkdocs/mkdocs) - mkdocs
- [gitlab-ci](https://docs.gitlab.com/ee/ci/) - GitLab ci

## Acknowledgements <a name = "acknowledgement"></a>

- Inspiration
- References
  - [Markdown Reference](https://www.markdownguide.org/)
