# Coded Music Scoring with Sonic Pi

## Table of Contents
1. [Introduction](#introduction)
2. [How It Works](#how-it-works)
3. [Getting Started](#getting-started)
    - [Prerequisites](#prerequisites)
    - [API Setup](#api-setup)
    - [Installation](#installation)
    - [Working with Samples](#working-with-samples)
4. [Configuration](#configuration)
5. [Running the System](#running-the-system)
   - [Web Application](#web-application)
   - [Command Line Interface](#command-line-interface)
6. [Output Files](#output-files)
7. [About](#about)

---

## Introduction

This project is a Multi-Agent System (MAS) that creates music by generating Sonic Pi code. Using generative AI, it produces complete song structures, arrangements, and lyrics based on your preferences. The system generates instrumental compositions (you can add vocals through samples if desired).

## How It Works

[Watch Music Agent in action](https://www.youtube.com/watch?v=rcfCjKbLkK0)

The system uses specialized AI assistants, each handling different aspects of music creation. These agents work together through multiple phases, starting from your initial input and preferences.

The system includes the following specialized roles (configured in `ArtistConfig.json`):
- **Artist** - Overall creative direction
- **Composer** - Musical composition
- **Songwriter** - Lyrics and songwriting
- **Arranger** - Musical arrangement
- **Sonic PI Coder** - Code generation for Sonic Pi
- **Sonic PI Reviewer** - Code review and quality assurance
- **Sonic PI Mix Engineer** - Audio mixing
- **Master Engineer** - Final mastering
- **Music Publisher** - Final output generation

The workflow consists of four main phases:

### 1. Design Phase
Initial planning and conceptualization of the song.

### 2. Creation Phase
Generation of the Sonic Pi script, initial playback testing, iterative review (by agents or humans), and code refinement.

### 3. Mastering Phase
Audio mixing and mastering to polish the final track.

### 4. Publishing Phase
Final output generation including the Sonic Pi script file, album cover artwork, booklet, and optional audio recording.

The final output includes a booklet with album cover art, lyrics, technical information, and the complete Sonic Pi code file (`.rb` format).

## Getting Started

### Prerequisites

- **Sonic Pi**: Required to run the generated `.rb` files. Download from: https://sonic-pi.net/
- **Python**: Version 12 is currently supported

### API Setup

The system supports multiple AI providers. You can use OpenAI, Anthropic, or Azure OpenAI APIs. Note that Anthropic doesn't support image generation, so album covers won't be created when using that provider.

Set your API keys as environment variables or in the configuration file:

**OPENAI_API_KEY:**
- On macOS/Linux:
  ```bash
  export OPENAI_API_KEY='<your_api_key>'
  ```
- On Windows (PowerShell):
  ```bash
  $env:OPENAI_API_KEY='<your_api_key>'
  ```

**ANTHROPIC_API_KEY:**
- On macOS/Linux:
  ```bash
  export ANTHROPIC_API_KEY='<your_api_key>'
  ```
- On Windows (PowerShell):
  ```bash
  $env:ANTHROPIC_API_KEY='<your_api_key>'
  ```

**AZURE_OPENAI_API_KEY:**
- On macOS/Linux:
  ```bash
  export AZURE_OPENAI_API_KEY='<your_api_key>'
  ```
- On Windows (PowerShell):
  ```bash
  $env:AZURE_OPENAI_API_KEY='<your_api_key>'
  ```

Alternatively, you can set these in `App/static/config/settings.json`.

### Installation

```bash
# Clone the repository
git clone https://github.com/takakhoo/Coded_Music_Scoring_SonicPI.git

# Navigate to the project directory
cd Coded_Music_Scoring_SonicPI

# Install Python dependencies
pip install -r requirements.txt
```

**Note:** If you plan to use Anthropic's API, you'll also need to install Rust: https://www.rust-lang.org/tools/install

### Working with Samples

You can add your own audio samples to the `Samples` folder. The folder comes with a base set of samples, but you can easily extend it by adding new sample files.

To use samples in your compositions, the system needs metadata describing each sample. This information helps the AI agents make better musical decisions.

When you add new sample packs, regenerate the metadata by running:

```bash
python SampleMedataListing.py
```

This generates a JSON file with sample metadata in the following format:

```json
{
    "Filename": "Synth/Prophet REV2 KEYS Echo Low - C.wav",
    "Duration": 3.2,
    "BPM": 161.5,
    "Key": "A minor",
    "Vibe": "The track has a Energetic tempo at 161 BPM, featuring a warm and high energy sound. It feels soft and smooth with a A minor tonality.",
    "Tags": [
        "Energetic",
        "warm",
        "high energy",
        "soft and smooth",
        "A minor",
        "Whale vocalization",
        "Keyboard (musical)",
        "Piano",
        "Ukulele",
        "Music"
    ],
    "Description": "A warm, high energy track with a Energetic tempo and a A minor tonality.",
    "Track Type": "Instrumentals Only"
}
```

The system uses Yamnet for sample classification. More details can be found in the [Yamnet README](App/inc/yamnet-tensorflow2-yamnet-v1/README.md).

## Configuration

Set your **OPENAI_API_KEY** in `AgentConfig/mITyJohn/ArtistConfig.json` if you haven't set it as a system environment variable. You can adjust other settings in this file as needed.

The system comes with several artist configurations:

- **Basic**: Standard music creation workflow
- **Eval**: Includes Sonic Pi code evaluation via a running Sonic Pi instance
- **Full**: Includes code evaluation and automatic recording (Windows only currently)
- **Art**: Only generates album cover artwork (no song generation)

For the Eval and Full configurations, you'll need additional setup:

1. **Launch Sonic Pi** on your machine
2. **Configure connection**: Update `ArtistConfig.json` with the correct `sonic_pi_IP` and `sonic_pi_port` (found in Sonic Pi IDE via menu > IO). Make sure incoming OSC messages are allowed.
3. **Set up the listener**: Copy and run this code in Sonic Pi (must be running before starting the system):

```ruby
live_loop :listen do
  use_real_time
  script = sync "/osc*/run-code"
  
  begin
    eval script[0]
    osc_send '127.0.0.1', 4559, '/feedback', 'MusicAgent Code was executed successfully'
  rescue Exception => e
    osc_send '127.0.0.1', 4559, '/feedback', e.message
  end
end
```

Alternatively, you can load `SonicPi/Setup/recording.rb` directly in Sonic Pi.

4. Once running, you'll see the listener active in your Cues panel, enabling Sonic Pi to execute your generated code and send feedback back to the system.

## Running the System

You can run the system in two ways: through a web application or via the command line.

### Web Application

The easiest way to get started is using the `start_musicagent.bat` script in the main folder, which launches both the backend and frontend.

You can also run them separately:

**Backend:**
```bash
cd App && python app.py
```

**Frontend:**
```bash
cd Frontend && npm run serve
```

The web interface visualizes the music creation process and lets you interact with the AI agents. You can view the conversation history with different agents, check generated Sonic Pi code versions, and even send code directly to the Sonic Pi IDE.

For more details on using the web application, see the [Music Agent App README](App/README.md).

### Command Line Interface

For command-line usage, run:

```bash
python run.py
```

You'll be prompted to:
- Choose an AI model (e.g., "gpt-3.5-turbo", "gpt-4")
- Provide song details: name, duration, style
- Optionally specify additional requests like chord progressions or musical influences

Example Sonic Pi code can be found in the `SonicPi/Examples` folder.

## Output Files

The system generates the following files in the `Songs` folder, organized in subdirectories named after each track:

- **Track File (`.rb`)**: The Sonic Pi code file. Load this in Sonic Pi to play your track.
  - When using the "Full" configuration, a WAV recording file is also created automatically.
- **Booklet**: Contains the album cover image, lyrics, and technical information about the track setup.
- **Log File**: Complete logging of the generation process. Useful for debugging if code is lost or incomplete.

If you're using the Full configuration and have your recording device properly configured (Windows only currently), recordings are made automatically.

## About

This project is an open-source Multi-Agent System for generating music with Sonic Pi using generative AI. It's available on GitHub for the community to use, contribute to, and improve.

---

**Repository**: https://github.com/takakhoo/Coded_Music_Scoring_SonicPI
