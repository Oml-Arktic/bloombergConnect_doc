# Bloomberg-connect, CFit enviroment

## Overview
Bloomberg connector `bloomberg-connect` is a microservice running in the BBVA C-Fit environment. Its main task is retrieve realtime market data from Bloomberg B-Pipe servers and provide snapshots of this information to other microservices in the BBVA C-Fit environment through kafka.

---

## Features
- 
- 

---

## Instalation

### Prerequisites
To get the development environment running you need:
- **Visual Studio Code with Dev Container extension**
- **WSL2 (or linux environment)**
- **Git**

### Steps
1. Get Docker:
   ```bash
   sudo apt install docker.io
   ```
   For Docker with WSL2 must enable `Use the WSL 2 based engine`
   
2. Submit credentials:
   ```bash
   docker login artifactory.globaldevtools.bbva.com:443
   ```
   - USER is name from your email account name@bbva.com (basically without @bbva.com)
   - PASS is the 'Reference Token' from Artifactory (https://artifactory.globaldevtools.bbva.com/ui/user_profile)
  
3. Create a file called `.env` under .devcontainer/, with two env variables:
   ```
   ARTIFACTORY_USER=<name_without_@bbva.com>
   ARTIFACTORY_PASS=<artifactory_reference_token>
   ```
   
4. Modify your `~/.gitconfig` file by adding these two lines under `[user]` section (your git user by default):
   ```
   [includeIf "gitdir:/code/bbva/"]
   path = .gitconfig-bbva
   ```

5. Create `~/.gitconfig-bbva` with this content:
   ```
   [user]
        name = `<your_name>` (e.g. John Doe)
        email = `<bbva_email>`
   ```
   This will basically overwrite your default git user with the one from BBVA when in dir /code/bbva, essentially when in the dev container (or in the same dir from your host WSL machine).
   
5. Run:
   ```bash
     chmod 600 ~/.gitconfig* && chmod -x ~/.gitconfig*
   ```

---

### Mock enviroment

## Usage

### Command-Line Interface (CLI)
1. Place your audio file in the `input` folder.
2. Run the software:
   ```bash
   python transcribe.py --file input/audio_file.wav --language en
   ```
3. The transcription will be saved in the `output` folder as a `.txt` file.

### Parameters
- `--file`: Path to the audio file.
- `--language`: Language code for transcription (default: `en`).
- `--format`: Output format (`txt` or `srt`).

---

## Dependencies
- 
- 

To install dependencies:
```bash

```

---

## Roadmap
- 
- 

---

## Contributing


---

## License


---
