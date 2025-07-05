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
