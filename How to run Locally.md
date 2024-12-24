# RUN SERVICES LOCALLY

## Requirement

### HZERO Source code

- Request the repository access by sending your github email address to Azka
- Create ur own branch and service ;D
- Try to tweak this function to process ur file type (maybe theres some escape character or sumn)
  ![image](https://github.com/user-attachments/assets/bfc89671-b781-4222-8a3d-0800bab7460c)


### Ngrok **—** https://dashboard.ngrok.com/get-started/setup/windows

- Create Account

- Download the ngrok folder put it in C:/

  ![ngrok-location](https://github.com/user-attachments/assets/f342582e-5998-4477-a681-dae10db58dbf)

- Get Ngrok auth from dashboard
  ![ngrok-auth](https://github.com/user-attachments/assets/e9cae71a-c0ae-4b23-86ad-f8772bd61e68)

- Run that command line inside ngrok
  ![ngrok-config](https://github.com/user-attachments/assets/2167242e-24b6-4d1f-abbe-933d666eee57)

- Then run: `ngrok http 172.22.4.112:8080`
  ![ngrok-run](https://github.com/user-attachments/assets/4089b4ae-b9c1-4c91-a802-a984cfc58976)

  ![ngrok-url](https://github.com/user-attachments/assets/e9655a3b-99f7-4306-b02a-ac2c0af56065)

- Use that ngrok URL for the ngrok field in https://hive-bridge.vercel.app/
  (U may need to add your service name after the ngrok url.
  e.g. https://dc95-103-118-101-82.ngrok-free.app/hive-bridge/v1/0)


