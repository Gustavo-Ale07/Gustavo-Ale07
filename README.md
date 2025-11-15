name: Generate snake animation

on:
  schedule:
    - cron: "0 0 * * *" # Executa diariamente à meia-noite
  workflow_dispatch:
  push:
    branches:
      - main

# Permissão de escrita para o token de segurança
permissions:
  contents: write

jobs:
  generate:
    runs-on: ubuntu-latest
    timeout-minutes: 5

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4

      # GERA O SNAKE GAME (Salva em dist/snake.svg)
      - name: Generate snake.svg
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: dist/snake.svg?palette=github-dark
          github_token: ${{ secrets.GITHUB_TOKEN }} 
      
      # FAZ O PUSH para o branch 'output'
      - name: Push snake.svg to the output branch
        uses: crazy-max/ghaction-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist # CORREÇÃO: Puxa o arquivo da pasta 'dist'
          # O parâmetro 'files' FOI REMOVIDO
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
