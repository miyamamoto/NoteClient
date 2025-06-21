# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

NoteClient is a Python library that automates article posting to the Note platform (note.com) using Selenium WebDriver. It converts Markdown content to Note's format and publishes articles with automatic image selection and tagging.

## Installation & Setup

```bash
pip install -e .
```

**Dependencies:**
- `selenium>=4.12.0` - Web automation framework  
- `janome>=0.5.0` - Japanese text tokenizer for keyword extraction
- `webdriver-manager>=3.8.0` - Automatic WebDriver management
- Firefox browser required for Selenium automation

**Cross-Platform Support:**
- Works on Mac ARM (Apple Silicon) and Intel architectures
- Automatically handles geckodriver installation via webdriver-manager
- Falls back to system geckodriver if available

## Core Architecture

### Main Components

**note_client/note_client.py:26-382** - `Note` class with single method `create_article()` that:
1. Authenticates with Note platform using credentials
2. Parses Markdown content from file or text parameter
3. Converts Markdown syntax to Note's editor format via Selenium
4. Extracts keywords from title using Janome tokenizer
5. Automatically selects article header image based on keywords
6. Publishes article or saves as draft

### Supported Markdown Features

- Headers: `##` (major), `###` (minor)
- Numbered lists: `1. item`  
- Bullet points: `- item` or `> item`
- Horizontal rules: `---`
- Block quotes: content between `\`\`\``
- URLs: automatically detected and formatted

### Key Implementation Details

- Uses Firefox WebDriver with headless option
- Implements sleep delays for UI interactions
- Japanese text processing for automatic keyword extraction
- XPath-based element selection for Note's web interface
- Random image selection from search results if no index specified

## Usage Pattern

```python
from note_client import Note

note = Note(email="email", password="password", user_id="user_id")
result = note.create_article(
    title="Article Title",
    file_name="content.txt",  # or text="content"
    input_tag_list=["tag1", "tag2"],
    image_index=0,  # optional
    post_setting=True,  # False for draft
    headless=True
)
```

## Development Notes

- No test framework currently implemented
- No linting configuration present
- Browser automation requires stable internet connection
- XPath selectors are brittle and may break with Note platform UI changes
- Uses hardcoded sleep delays which may need adjustment