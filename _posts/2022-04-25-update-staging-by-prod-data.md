---
title: "Trasfer all data from production stage on Heroku to staging stage (Postgres DB + S3 bucket)"
author: DTOSNER
date: 2022-04-25 19:00:00 -0000
categories: [Heroku]
tags: [rails, postgres, heroku, wsl2, aws s3]
---

This topic covers how to transfer data from production stage on Heroku to staging stage on Heroku. Data is composed by Postgres DB and photos on Amazon S3 service.


## Just commands
---

> --dryrun run command virtualy. For real use delete this option in commands.
{: .prompt-info }

```console
aws s3 sync s3://staging-app-bucket C:\Users\snuffle_truffle\Downloads\prod_app_images --dryrun --profile s3_snuffle_truffle
aws s3 rm s3://staging-app-bucket --recursive --dryrun --profile s3_snuffle_truffle
aws s3 sync s3://prod-app-bucket s3://staging-app-bucket --dryrun --profile s3_snuffle_truffle
```
```console
heroku pg:backups:restore "https://snuffle-truffle.s3.eu-west-1.amazonaws.com/db-import/prod_6661c6a54e1d855c6bebec97aa205b9516ec3d2c_202204251011?response-content-disposition=inline&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCWV1LXdlc3QtMiJIMEYCIQCKPEuAadtPTSABbCRN5ADCekDl3GUFhBZds05hS0u%2BtwIhALemKhtXDSY6tcxwCKwx8Neyr2Bwax3O2US%2F1NJS2dyHKoQDCIr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQAxoMMjk1NTM2Njc0NzcyIgze7F0dj%2Fagd8wzb54q2AL1oFnbr3AIFY7%2BwihFp%2B9z5ZcBXhd5UggsHYN0572tx8825sH4dAjCEzBgFOd3JYYaRkDX5EWydcOpEAKLyiNam9agmZkzEU%2FyRpLmSSms8sEuFT1ZyvIFX4EDPjtO1dMAhF5qxQvsSdGbYq0rsL7g7jPar2vXmuWtx3QPWy%2B9BqVkzQvhn6%2BBSTDUav8DlDUj2iaoLQvRmZmbJJ5cqzYl5DSyGiiwzQGvpuPUWZjhdFyTFZZkBIYSh3YUnSQDFoaQT9Kok%2BNGs6iDMEzZLeWeLudXxnYpAThzb1q16VQfwInGpW3%2BRSuWUN5nuKacUejZINmGRi5Sjh9FzxBZmvfEgtWigw4E5tf8XpxhJw1ecdaY9Gt4UOvDKJmF3iFvhRKbVIdh%2FLEc2UuU4iQbf6%2FkQnsxBm%2Be8zM3PPaAf%2BBf4AQ%2FJaGWwnwDrtCFk7Y8OyvBGbL27JksNzD9tZmTBjqyAot0luC2%2FCn%2BtmE0WIVDZ%2F8VRjSdiWZEluznSUmV9jS%2B%2BB3V6AKEsXskZXxLnhIzmPMpqGqtDe3v%2BC50iEaZkK%2FW7ilXExJ%2Fj7SnqnerfXEHe%2FVSlAeetyIlpfDh%2FCFDcIxle%2Bnh0aTSp6vEhMtHwwXGk0vIbqbptu6Q5d7TTK0VexqXv8UcXhH8c6kblALTxSaZLVNuVgdfxFYQfLV9IQaQMZ1XoI3TDzJ9uBaHsed02%2F7jJgJBcNgC7q7wWYn9jTcQghlzznJBMjyw%2FQ%2BfRUF54zLYmPcDtr7lobLjHz04mebQEquiKGt2qxQj2iP5wj9dGUp2xT3ezJufQaVpUDKr6VziDnPWmyE3x6pYVOo04BOSVID9NQRQHlqPSX9y18I7i%2BBfGIjli59mMq1wNlx8og%3D%3D&X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Date=20220425T100132Z&X-Amz-SignedHeaders=host&X-Amz-Expires=3600&X-Amz-Credential=ASIAUJT23Y7KH44YBWWR%2F20220425%2Feu-west-1%2Fs3%2Faws4_request&X-Amz-Signature=ea15dbc7818915882a3e2543ec3e1afdc5799faf61beb151d3523db65c86f449" DATABASE_URL --app api-alpha-rem-lexxus
```
---
---

## Step by step
---

### Create backup of data on S3

Download all data from S3 bucket for staging stage on Heroku.
```console
aws s3 sync s3://staging-app-bucket C:\Users\snuffle_truffle\Downloads\prod_app_images --dryrun --profile s3_snuffle_truffle
```
