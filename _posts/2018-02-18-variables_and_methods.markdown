---
layout: post
title:      "Object Orientation in Ruby"
date:       2018-02-18 22:52:38 -0500
permalink:  variables_and_methods
---

I will discuss the topic of object orientation in Ruby and the basic principles of object-oriented programming in this blog post. Object-oriented programming is a paradigm in computer programming whereby data is represented by objects. Objects are representations of real-world objects, such as artists, songs, and genres.

Objects belong to a *class*, which is a blueprint that defines how objects that belong to the same class must be created. Classes define information, particularly attributes and methods, that are common to all of its objects. An example of a class would be songs in general. Any particular songs would be considered *instances* of the class. Examples of particular instances of songs include “Thriller”, “Hey Jude”, and “We Will Rock You”.

Additionally, objects also contain *attributes* which refer to data or identifying information that pertain to the particular object in question. Examples of attributes in the case of songs would include a name, title, artist, and genre. Objects also contain *methods* which refer to logic and enables an object to execute functionality or perform a certain behavior. Examples of methods would include the self.find_by_name or self.find_or_create_by_name methods as demonstrated in the Songs controller below. See the following code:


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

The attributes of song that are defined in the Songs controller include name, artist, and genre. Note that name is defined upon initialization of a new Song instance which occurs in the initialize method. The artist= and genre= methods are setter methods, which are used to assign the values of artist and genre to the particular instance of Song that the methods are called upon. The self.find_by_name or self.find_or_create_by_name are *class methods* which are used to either locate the Song instance that has a name that matches the name that is being passed in as an argument or create a new Song instance if the song does not already exist in the class. 

There are also some attr_accessor and attr_reader methods in the Songs controller that are worthy of mention. The attr_accessor is a macro for creating getter and setter methods for a particular attribute. As mentioned earlier, the *setter method* is used to assign a value to a particular attribute which will be associated to the instance of Song that the method is called upon. The *getter method*, on the other hand, is concerned with returning the value of the attribute that was defined in its corresponding setter method. Macros are considered a form of metaprogramming and are powerful because they will automate certain tasks and reduce the amount of code that is needed to accomplish a task. Macros, therefore, help keep the code in the application DRY.

As with any language, learning a computer programming language requires an understanding of the basic fundamentals of the language, including variables, data structures, syntax, and functions. Object-oriented programming is important because it allows for the interaction between objects through the use of methods, which enables a higher level of complexity in the application. Moreover, learning a computer programming language is a very powerful skill that will provide you with the ability to create computer programs, websites, mobile applications, games, and other software. 

Thank you for reading this blog post. Stay tuned for future posts on other computer programming fundamentals! 


