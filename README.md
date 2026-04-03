# Git tools for Drupal projects


This repository facilitates the integration of tools such as code quality checks, code security checks before during development and check in the git repository, on the developer's machine. 

Currently the following features are supported:


1. pre-commit


## pre-commit


The pre-commit hook is executed before the code is committed in the repository and it allows repositories to setup the following features:


### gitleaks

[Gitleaks](https://gitleaks.io/) tool scans the code for API keys and other secrets stored in the repository code base and history. 

This is integrated in the `pre-commit` hook and currently works only with DDEV environment configuration and `composer` based projects. To install support for `pre-commit` hook and scan for gitleaks before commit, use the following configuration in `composer.json`, under `scripts` key:


```json
{
	"scripts": {
		"post-install-cmd": [
            "curl -s -o .git/hooks/pre-commit https://raw.githubusercontent.com/eaudeweb/drupal-git-hooks/refs/heads/main/pre-commit && chmod +x .git/hooks/pre-commit; true",
            "curl -s -o .gitleaks.toml https://raw.githubusercontent.com/eaudeweb/drupal-git-hooks/refs/heads/main/.gitleaks.toml; true",
            "curl -s -o .ddev/web-build/Dockerfile.gitleaks  https://raw.githubusercontent.com/eaudeweb/drupal-git-hooks/refs/heads/main/.ddev/Dockerfile.gitleaks; true",
            "curl -s -o .ddev/commands/web/gitleaks https://raw.githubusercontent.com/eaudeweb/drupal-git-hooks/refs/heads/main/.ddev/gitleaks; true"
        ]
    }
}
```

Additional `gitleaks` configuration - If you have false-positives in your project, you can whitelist them individually using the `.gitleaksignore` file. See example below:

```
# Gitleaks ignore file
# Format: commit:path:rule:line
particle/.travis.yml:aws-access-token:33
particle/source/default/_patterns/05-pages/press-page/_press-page.twig:generic-api-key:23
```

Ignore gitleaks-specific files in ``.gitignore`:

```
.gitleaks.toml
.ddev/commands/web/gitleaks
.ddev/web-build/Dockerfile.gitleaks
```
