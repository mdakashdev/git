# API Tool

## Postman

- postman open kore collection create
- add request create for endpoint
- method select post/get/etc
- Header theke `Accept: application/json` set kora sathe jodi token thake tobe `Authorization: Bearer YOUR_TOKEN`
- Body theke form-data / raw theke json formate a data pass kora. and test kora


### authenticated endpoint 

1. ami /me (profile) er data chai, tahole obossoit jei user dekhbe take authenticated hote hobe

- setar jonno age /login api call kore token nite hobe, like - `12|duiJZbCEQQenu0RUjlSuFmuJW4KJwjRmfSME3OEPfee3bb70`
- sei token diye /me call korte hobe
- rules: postman headers a, `Accept: application/json` and `Authorization: Bearer 12|duiJZbCEQQenu0RUjlSuFmuJW4KJwjRmfSME3OEPfee3bb70`