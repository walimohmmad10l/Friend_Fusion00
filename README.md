Friend Fusion - A Full-Stack Social Media Application
Friend Fusion is a comprehensive full-stack social media application built to showcase modern web development practices using the MERN stack (MySQL, Express, React, Node.js). This project serves as a portfolio piece demonstrating skills in backend API development, dynamic frontend user interfaces, and database management. It is a fully functional platform designed with a focus on user interaction and real-time updates.


✨ Core Features
Secure User Authentication: Robust user registration and login system implemented with JSON Web Tokens (JWT) for secure, stateless authentication.

Dynamic Post Feed: A real-time timeline displaying posts from followed users, allowing for content discovery.

Post & Content Management: Users can create text-based posts, upload images, and manage their own content.

Real-Time Social Interactions: Functionality to like/unlike posts and engage in discussions through a comment system.

Customizable User Profiles: Users can view and update their profiles, including cover photos, profile pictures, and personal details.

Follow/Unfollow System: A social graph where users can follow and unfollow others to curate their content feed.

Efficient Data Handling: Leverages React Query for optimized client-side data fetching, caching, and state synchronization.

Modern UI/UX: Features a responsive design with a switchable Dark/Light mode for enhanced user experience.

🛠️ Tech Stack
Frontend: React, React Router, Context API, Sass

State Management: React Query for server state, Context API for UI state.

Backend: Node.js, Express.js

Database: MySQL

API & Data: RESTful API, Axios for HTTP requests

Authentication: JSON Web Tokens (JWT), bcrypt.js for password hashing, Cookie-Parser

🚀 Getting Started
To get a local copy up and running, follow these simple steps.

Prerequisites
Make sure you have the following software installed on your machine:

Node.js (which includes npm)

MySQL Server

Installation
Clone the repository

bash
git clone https://github.com/walimohmmad10l/Friend_Fusion00.git
cd Friend_Fusion00
Set up the Backend (API)

Navigate to the api directory and install dependencies.

bash
cd api
npm install
Create a .env file in the api directory and add your database credentials.

text
DB_HOST=localhost
DB_USER=your_mysql_username
DB_PASSWORD=your_mysql_password
DB_NAME=social
JWT_SECRET=your_super_secret_key
Set up the Database

Connect to your MySQL server (e.g., using MySQL Workbench or another client).

Import the table structure from the api/schema.sql file. This will create the social database and all necessary tables. You can do this via a tool's import feature or by executing the script directly.

Set up the Frontend (Client)

From the root directory, navigate to the client folder and install dependencies.

bash
cd ../client
npm install
Running the Application
Start the Backend Server

In the api directory, run:

bash
npm start
The API server will start on http://localhost:8800.

Start the Frontend Application

In the client directory, run:

bash
npm start
The application will be accessible at http://localhost:3000.


-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



--
-- Create a new database named 'social' if it doesn't exist
--
CREATE DATABASE IF NOT EXISTS social;
USE social;

--
-- Table structure for table `users`
--
CREATE TABLE `users` (
  `id` int NOT NULL AUTO_INCREMENT,
  `username` varchar(45) NOT NULL,
  `email` varchar(45) NOT NULL,
  `password` varchar(200) NOT NULL,
  `name` varchar(45) NOT NULL,
  `coverPic` varchar(100) DEFAULT NULL,
  `profilePic` varchar(100) DEFAULT NULL,
  `city` varchar(45) DEFAULT NULL,
  `website` varchar(45) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `username_UNIQUE` (`username`)
) ENGINE=InnoDB;

--
-- Table structure for table `posts`
--
CREATE TABLE `posts` (
  `id` int NOT NULL AUTO_INCREMENT,
  `description` varchar(200) DEFAULT NULL,
  `img` varchar(200) DEFAULT NULL,
  `userId` int NOT NULL,
  `createdAt` datetime DEFAULT CURRENT_TIMESTAMP,
  PRIMARY KEY (`id`),
  KEY `userId_idx` (`userId`),
  CONSTRAINT `posts_userId_fk` FOREIGN KEY (`userId`) REFERENCES `users` (`id`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB;

--
-- Table structure for table `comments`
--
CREATE TABLE `comments` (
  `id` int NOT NULL AUTO_INCREMENT,
  `description` varchar(200) NOT NULL,
  `createdAt` datetime DEFAULT CURRENT_TIMESTAMP,
  `userId` int NOT NULL,
  `postId` int NOT NULL,
  PRIMARY KEY (`id`),
  KEY `commentUserId_idx` (`userId`),
  KEY `postId_idx` (`postId`),
  CONSTRAINT `comments_postId_fk` FOREIGN KEY (`postId`) REFERENCES `posts` (`id`) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT `comments_userId_fk` FOREIGN KEY (`userId`) REFERENCES `users` (`id`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB;

--
-- Table structure for table `likes`
--
CREATE TABLE `likes` (
  `id` int NOT NULL AUTO_INCREMENT,
  `userId` int NOT NULL,
  `postId` int NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `likes_UNIQUE` (`userId`,`postId`),
  KEY `likeUserId_idx` (`userId`),
  KEY `likePostId_idx` (`postId`),
  CONSTRAINT `likes_postId_fk` FOREIGN KEY (`postId`) REFERENCES `posts` (`id`) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT `likes_userId_fk` FOREIGN KEY (`userId`) REFERENCES `users` (`id`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB;

--
-- Table structure for table `relationships`
--
CREATE TABLE `relationships` (
  `id` int NOT NULL AUTO_INCREMENT,
  `followerUserId` int NOT NULL,
  `followedUserId` int NOT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `relationship_UNIQUE` (`followerUserId`,`followedUserId`),
  KEY `followerUser_idx` (`followerUserId`),
  KEY `followedUser_idx` (`followedUserId`),
  CONSTRAINT `relationships_followed_fk` FOREIGN KEY (`followedUserId`) REFERENCES `users` (`id`) ON DELETE CASCADE ON UPDATE CASCADE,
  CONSTRAINT `relationships_follower_fk` FOREIGN KEY (`followerUserId`) REFERENCES `users` (`id`) ON DELETE CASCADE ON UPDATE CASCADE
) ENGINE=InnoDB;
