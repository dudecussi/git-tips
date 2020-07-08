# Dicas e Truques práticos para o GIT

## GIT-FTP

### Integração para auto-publicação de sites em hospedagens convencionais, por meio de FTP

1. Instalar o git-ftp

https://github.com/git-ftp/git-ftp

2. Inicializar o **repositório remoto**

`git ftp init --user {user} --passwd '{password}' --verbose --insecure sftp://{host}:{port}/{absolute_path}`

3. Configurar a integração

`{PROJECT}/.git/post-commit`

Por meio da configuração abaixo, o simples fato de realizar commits nos branchs **master** e **testing** realizará o deploy para seus respectivos repositórios.
Caso não queira gerar nenhuma alteração nos ambientes, basta desenvolver em um branch separado como **dev**, por exemplo.
```bash
#!/bin/sh

branchPath=$(git symbolic-ref -q HEAD)
branch=${branchPath##*/}

echo "Head: $branchPath";
echo "Current Branch: $branch";

echo ""

if [ "master" == "$branch" ]; then

    echo "Updating production files:"
    # git ftp push --user {prod:user} --passwd '{prod:password}' --verbose --insecure sftp://{prod:host}:{prod:port}/{prod:absolute_path}

fi


if [ "testing" == "$branch" ]; then

    echo "Updating testing files:"
    # git ftp push --user {test:user} --passwd '{test:password}' --verbose --insecure sftp://{test:host}:{test:port}/{test:absolute_path}

fi


if [ "dev" == "$branch" ]; then

    echo "Updating local files:"
    # nothing to do

fi

echo "\n=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-=-\n"
```
