# summon-credstash

This is naive BASH-based [`credstash`](https://github.com/fugue/credstash) provider for [`summon`](https://github.com/cyberark/summon).  
Created for as tutprial / reference for making simple Summon providers.

## Installation
[Set summon-cerberus as your Summon provider](https://github.com/cyberark/summon#flags)
> `-p, --provider` specify the path to the provider summon should use
> If the provider is in the default path, /usr/local/lib/summon/ you can just provide the name of the executable. If not, use a full path.

## Usage

### Simple
```bash
# secrets.yaml
---
USERNAME: default_user
PASSWORD: !var dev-credstash/password

$ summon -p summon-credstash -f secrets.yaml \
         cat @SUMMONENVFILE
USERNAME=default_user
PASSWORD=supersecret
```

### With variable substitution
```bash
# secrets_with_sub.yaml
---
USERNAME: default_user
PASSWORD: !var $ENV-credstash/password

$ summon -p summon-credstash -f secrets_with_sub.yaml \
         -e ENV=dev cat @SUMMONENVFILE
USERNAME=default_user
PASSWORD=supersecret
```

### With environment selection
```bash
# secrets_with_env.yaml
---
defaults: &defaults
  USERNAME: default_user
  PASSWORD: default_password
dev:
  <<: *defaults
  PASSWORD: !var dev-credstash/password

$ summon -p summon-credstash -e dev \
         -f secrets_with_sub.yaml cat @SUMMONENVFILE
USERNAME=default_user
PASSWORD=supersecret
```
