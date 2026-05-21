# Time Based OTP

## Consists of two main components:
1. **Seed**: Server generates a secret key for each user which is shared to the client's TOTP app.
2. **Moving part**: This component changes after every few seconds. Usually 30 seconds. UNIX time stamp is used generally for this.
   

## Stages of TOTP Auth:
1. ### Initialization:
    * Server generates secret key for each user.
    * This secret key is encoded (usually using QR code) and shared with the user's TOTP app.
    * The client's TOTP scans this QR code and stores the secret key.

2. ### TOTP Generation:
    * The TOTP takes the current time and divides it by a time step and calulates the number of time steps since a reference time (usually UNIX epoch time - 1 Jan 1970).
    * It then uses a hashing algorithm generally HMAC-SHA-1, combining the seed and the time steps to generate a hash value.
    * A portion of this hash value is used to generate a 6 to 8 digits code, which is the TOTP.

3. ### Authentication:
   * The user enters this TOTP and it is passed to the server.
   * The server already has the secret key. It repeats the same process to generate the TOTP.
   * The server then comapares it with the user entered TOTP.

4. ### Verification:
   * If both the OTPs match, the user is authenticated.
   * If the OTPs don't match, then access is denied. It may occur due to entering wrong TOTP or expired TOTP.

## Note:
1. The server usually check the user entered TOTP with TOTPs generated from a previous and next time step to avoid time drift issue.