
# topradio - Radio Station Management System

## Overview

**topradio** is a service designed to manage multiple radio stations. The project is organized into several applications that separate concerns for easy maintainability and scalability. The key functionality includes managing songs, artists, radio programming, scheduling, playlists, and scraping external sources to import playlists.

---

## Project Structure

The project is built using **Django** and is structured in a modular way with the following applications:

### **1. Library** (Handles Music Data)
- **Purpose:** This app stores all songs, albums, and related metadata.
- **Key Models:**
  - **Song:** Contains information about songs, including the song's title, artist(s), duration, genre, category, etc.
  - **Artist:** Stores data about the artists, composers, lyricists, and any collaborators.
  - **Album:** Contains album metadata (optional).
  - **Category/Genre:** Defines song categories (e.g., Gold, New, Current) and genres (e.g., Pop, Rock).
  
**Example Models:**
```python
class Song(models.Model):
    title = models.CharField(max_length=255)
    artist = models.ManyToManyField('Artist')
    genre = models.ForeignKey('Genre', on_delete=models.SET_NULL, null=True)
    category = models.CharField(max_length=50, choices=CATEGORY_CHOICES)
    duration = models.DurationField()  # Duration in minutes:seconds
    composer = models.ManyToManyField('People.Composer', blank=True)
    lyricist = models.ManyToManyField('People.Lyricist', blank=True)
```

---

### **2. People** (Handles People Data)
- **Purpose:** This app stores information about people involved in music creation, including artists, composers, lyricists, arrangers, and producers.
- **Key Models:**
  - **Artist:** Contains personal information about the artists, such as name, bio, etc.
  - **Composer:** Stores composers’ information.
  - **Lyricist:** Stores lyricists’ information.

**Example Models:**
```python
class Artist(models.Model):
    name = models.CharField(max_length=255)
    bio = models.TextField()
    birth_date = models.DateField()
```

---

### **3. Air** (Handles Radio Programming and Scheduling)
- **Purpose:** This app manages the radio station's schedule, program structure, and playlists.
- **Key Models:**
  - **Clock:** Represents the time slots for specific categories of songs (e.g., New, Deep, Gold).
  - **Playlist:** Contains the list of songs scheduled for broadcast.
  - **Program:** Represents a radio program that includes multiple playlists, potentially with additional metadata like scripts and announcements.

**Example Models:**
```python
class Clock(models.Model):
    time = models.TimeField()
    category = models.CharField(max_length=100)
    program = models.ForeignKey('Program', on_delete=models.CASCADE)

class Playlist(models.Model):
    name = models.CharField(max_length=255)
    songs = models.ManyToManyField('Library.Song')
    clock = models.ForeignKey('Clock', on_delete=models.CASCADE)
```

---

### **4. Scraper (Parser)** (Handles Playlist Scraping)
- **Purpose:** This app is responsible for scraping data from external platforms like Spotify, YouTube, and radio stations to gather playlists and song information for analysis or machine learning purposes.
- **Key Features:**
  - Scrape playlist data from various platforms.
  - Import scraped data into the database to use in the scheduling, analysis, and research models.

---

## Architecture Overview

The overall architecture is designed to be modular with each application focused on a specific area of functionality. This allows for easy extension and maintenance as the project grows.

### **Core Components:**

1. **Song Management (Library App):**
   - The `Library` app is responsible for storing and managing song-related information. Songs are linked to one or more artists and have additional metadata like genre, category, composers, and lyricists.

2. **Artist and People Data (People App):**
   - The `People` app stores the various entities related to the creation of music: artists, composers, lyricists, and producers. This separation allows flexibility in tracking different roles.

3. **Scheduling and Programming (Air App):**
   - The `Air` app handles the scheduling and broadcasting of content. Each radio station can have a program with categories, and each program can be broken down into playlists, which are created from the songs stored in the `Library` app.

4. **Playlist Scraping (Parser App):**
   - The `Parser` app scrapes external playlists from platforms such as Spotify and YouTube. This data is then processed and stored in the database for research, analysis, and machine learning tasks.

---

## Setup Instructions

1. **Clone the repository:**
   ```bash
   git clone https://github.com/your-username/topradio.git
   cd topradio
   ```

2. **Set up a virtual environment:**
   ```bash
   python3 -m venv venv
   source venv/bin/activate  # On Windows, use `venv\Scriptsctivate`
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Apply migrations:**
   ```bash
   python manage.py migrate
   ```

5. **Create a superuser (for admin access):**
   ```bash
   python manage.py createsuperuser
   ```

6. **Run the development server:**
   ```bash
   python manage.py runserver
   ```

---

## Development Notes

- **Coding Style:** Follow PEP8 guidelines for Python code and Django conventions.
- **Testing:** Ensure all new functionality is tested. We will use Django's test framework, and you should write tests for each app.
- **Branching Strategy:** We follow GitFlow for branching:
  - `master` for production-ready code.
  - `develop` for ongoing development.
  - Feature branches for specific tasks or bug fixes.

---

## Project Future Directions

- **Machine Learning:** We plan to integrate machine learning models to analyze playlists, genres, and song data to suggest programming schedules.
- **Streaming Integration:** Future updates may include integrating live streaming capabilities and more advanced scheduling features.

---

## Contributing

We welcome contributions from the community! Please follow these steps:
1. Fork the repository.
2. Create a new branch for your feature or bugfix.
3. Make your changes and commit them.
4. Create a pull request with a detailed description of your changes.

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## Acknowledgments

- Django for making web development easier.
- Various radio stations and music services that inspire this project.
