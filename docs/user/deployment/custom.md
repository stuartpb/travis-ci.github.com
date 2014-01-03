---
title: Custom Deployment
layout: en
permalink: custom/
---

You can easily deploy to your own server the way you would deploy from your local machine by adding a custom [`after_success`](/docs/user/build-configuration/#Define-custom-build-lifecycle-commands) step.

### FTP, SFTP, SCP, etc.

    env:
      global:
        - "DEPLOY_URL=ftp://user:password@example.com"
    after_success: |
      if [[ "$TRAVIS_PULL_REQUEST" == "false" ]]; then
        find . -type f -exec curl --ftp-create-dirs -T {} $DEPLOY_URL/{}
      fi

The deploy URL can also be [encrypted](/docs/user/encryption-keys/).

### Git

    env:
      global:
        - "DEPLOY_URL=git@example.com:repo.git"
    after_success: |
      if [[ "$TRAVIS_PULL_REQUEST" == "false" ]]; then
        chmod 600 .travis/deploy_key.pem # this key should have push access
        ssh-add .travis/deploy_key.pem
        git push $DEPLOY_URL
      fi

See ["How can I encrypt files that include sensitive data?"](/docs/user/travis-pro/#How-can-I-encrypt-files-that-include-sensitive-data%3F) if you don't want to commit the private key unencrypted to your repository.
