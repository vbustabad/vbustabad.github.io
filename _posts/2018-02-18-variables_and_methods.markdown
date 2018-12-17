---
layout: post
title:      "Object Orientation in Ruby"
date:       2018-02-18 22:52:38 -0500
permalink:  variables_and_methods
---

I will discuss the topic of object orientation in Ruby and the basic principles of object-oriented programming in this blog post. Object-oriented programming is a paradigm in programming whereby data is represented by objects. Objects are representations of real-world objects, such as artists, songs, and genres.

Objects belong to a class, which is a blueprint that defines how objects that belong to the same class must be created. Classes define information, such as attributes and methods, that are common to all of its objects. An example of a class would be songs in general. Note that classes must be capitalized when written in code (e.g. “Songs”). Any particular songs would be considered instances of the class. Examples of particular instances of songs include “Thriller”, “Hey Jude”, and “We Will Rock You”.

Additionally, objects also contain attributes which refer to data or identifying information that pertain to the particular object in question. Examples of attributes in the case of songs would include a name, title, artist and genre. Objects also contain methods which refer to logic and enables an object to execute functionality or perform a certain behavior. Examples of methods would include the self.find_by_name or self.find_or_create_by_name as shown in the Songs controller. See the code below: 

```
class Song
  attr_accessor :name
  attr_reader :artist, :genre

  @@all = []

  def initialize(name, artist = nil, genre = nil)
    @name = name
    self.artist = artist if artist
    self.genre = genre if genre
  end

  def artist=(artist)
    @artist = artist
    artist.add_song(self)
  end

  def genre=(genre)
    @genre = genre
    genre.songs << self unless genre.songs.include?(self)
  end

  def self.all
    @@all
  end

  def self.destroy_all
    @@all.clear
  end

  def save
    self.class.all << self
  end

  def self.create(name)
    song = Song.new(name)
    song.save
    song
  end

  def self.find_by_name(name)
    @@all.detect do |song|
      song.name == name
    end
  end

  def self.find_or_create_by_name(name)
    self.find_by_name(name) || self.create(name)
  end

  def self.new_from_filename(filename)
    song_array = filename.split(" - ")
    artist_name, song_name, genre_name = song_array[0], song_array[1], song_array[2].gsub(".mp3", "")
    artist = Artist.find_or_create_by_name(artist_name)
    genre = Genre.find_or_create_by_name(genre_name)
    new(song_name, artist, genre)
  end

  def self.create_from_filename(filename)
    Song.new_from_filename(filename).save
  end

end
```
