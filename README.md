# BookMyShow
database design of book my show

Sql schema 
-- Theater Table
CREATE TABLE Theaters (
    TheaterID INT PRIMARY KEY,
    TheaterName VARCHAR(100),
    Location VARCHAR(100)
);

-- Movie Table
CREATE TABLE Movies (
    MovieID INT PRIMARY KEY,
    Title VARCHAR(100),
    Language VARCHAR(50),
    Format VARCHAR(10),
    Certification VARCHAR(5)
);

-- Screen Table (Links screens to theaters and tech specs)
CREATE TABLE Screens (
    ScreenID INT PRIMARY KEY,
    TheaterID INT,
    ScreenName VARCHAR(50),
    AudioVisualTech VARCHAR(50),
    FOREIGN KEY (TheaterID) REFERENCES Theaters(TheaterID)
);

-- Show Table (The bridge linking everything)
CREATE TABLE Shows (
    ShowID INT PRIMARY KEY,
    MovieID INT,
    ScreenID INT,
    ShowDate DATE,
    ShowTime TIME,
    FOREIGN KEY (MovieID) REFERENCES Movies(MovieID),
    FOREIGN KEY (ScreenID) REFERENCES Screens(ScreenID)
);

# Sample Entries

INSERT INTO Theaters VALUES (1, 'PVR: Nexus', 'Forum Mall');

INSERT INTO Movies VALUES 
(101, 'Dasara', 'Telugu', '2D', 'UA'),
(102, 'Kisi Ka Bhai Kisi Ki Jaan', 'Hindi', '2D', 'UA'),
(103, 'Avatar: The Way of Water', 'English', '3D', 'UA');

INSERT INTO Screens VALUES 
(501, 1, 'Screen 1', '4K Dolby 7.1'),
(502, 1, 'Playhouse', 'Playhouse 4K');

INSERT INTO Shows VALUES 
(1, 101, 501, '2025-04-25', '12:15:00'),
(2, 102, 501, '2025-04-25', '13:00:00'),
(3, 103, 502, '2025-04-25', '13:20:00');

# Show Listing Query

SELECT 
    m.Title, 
    m.Language, 
    s.ShowTime, 
    sc.AudioVisualTech
FROM Shows s
JOIN Movies m ON s.MovieID = m.MovieID
JOIN Screens sc ON s.ScreenID = sc.ScreenID
JOIN Theaters t ON sc.TheaterID = t.TheaterID
WHERE t.TheaterName = 'PVR: Nexus' 
  AND s.ShowDate = '2025-04-25'
ORDER BY s.ShowTime ASC;