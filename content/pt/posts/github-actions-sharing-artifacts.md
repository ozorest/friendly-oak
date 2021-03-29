+++
title = "Github Actions: Compartilhando artefatos entre jobs"
description = ""
tags = [
    "github",
]
date = "2021-02-28"
categories = [
    "Github",
]
menu = "main"
+++

Esta vai ser uma dica rápida.

Outro dia configurando um pipeline no Github Actions, eu tive a necessidade de usar um arquivo gerado em um job em um outro job, que estavam usando diferentes sistemas operacionais.

É bem simples fazer isso, tudo que você precisa é destas duas actions: `upload-artifact` e `download-artifact`

Aqui está um exemplo:
```yaml
job1:

  runs-on: ubuntu-latest
  steps:
  - uses: actions/checkout@v1

  - run: mkdir -p dist

  - run: echo hello > dist/world.txt

  - uses: actions/upload-artifact@master
    with:
      name: hello-world-artifact
      path: dist/world.txt

job2:
  runs-on: macos-latest

  steps:
  - run: mkdir -p dist

  - uses: actions/download-artifact@master
    with:
      name: hello-world-artifact
      path: dist/world.txt

  - run: cat dist/world.txt
```

Código-fonte e documentação das actions:
- [upload-artifact](https://github.com/actions/upload-artifact)
- [download-artifact](https://github.com/actions/download-artifact)

**DICA BÔNUS**
Eu também tive a necessidade de fazer o download de um arquivo do Releases do Github, para isso eu usei esta action `dsaltares/fetch-gh-release-asset@master` de terceiros, mas a desvantagem é que esta action apenas roda em Linux (por isso que eu precisei copiar um artefato de um job em outro ;-) )

Aqui está um exemplo:
```yaml
uses: dsaltares/fetch-gh-release-asset@master
with:
  repo: "your-user/your-repo"
  version: "latest"
  file: "package.zip"
  target: "dist/package.zip"
  token: ${{ secrets.YOUR_TOKEN }} # Se o seu repo é privado, você precisa do access token
```

Código-fonte e documentação da action [dsaltares/fetch-gh-release-asset](https://github.com/dsaltares/fetch-gh-release-asset)

Isso é tudo, pessoal! Obrigado e fique sintonia para mais dicas.