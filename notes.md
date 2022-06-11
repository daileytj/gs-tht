Device: 2021 MacBookPro M1 Pro
OS: Monterey v12.3.1
Browser: Chrome v102.0.5005.61

there's a typo in the README,md instruction: DiasyUI => DaisyUI

npm run dev / npm start:

- returns 403 - Access to localhost was denied, you don't have authorization to view this page.
- using `http://127.0.0.1:5000/` works just fine.\*

> \* Well, it did for a bit

- Update sirv-cli to v2.0.2 and everything runs fine now on localhost
