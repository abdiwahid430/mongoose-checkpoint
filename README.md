// Import required modules
const mongoose = require('mongoose');
require('dotenv').config(); // Load environment variables from .env file

// Connect to MongoDB using the URI from .env file
mongoose.connect(process.env.MONGO_URI, { useNewUrlParser: true, useUnifiedTopology: true })
  .then(() => console.log('MongoDB connected...'))
  .catch(err => console.error('Could not connect to MongoDB:', err));

  // Define the Person Schema
const personSchema = new mongoose.Schema({
    name: { type: String, required: true }, // Name is required
    age: Number, // Age is optional
    favoriteFoods: { type: [String], default: [] } // Favorite foods as an array of strings
  });
  
  // Create the Person model
  const Person = mongoose.model('Person', personSchema);

  // Create a new person instance
const newPerson = new Person({
    name: 'John Doe',
    age: 30,
    favoriteFoods: ['Pizza', 'Pasta']
  });
  
  // Save the person to the database
  newPerson.save(function(err, data) {
    if (err) return console.error('Error saving data:', err);
    console.log('Person saved:', data);
  });

  // Array of people to create
const arrayOfPeople = [
    { name: 'Mary', age: 25, favoriteFoods: ['Burrito', 'Tacos'] },
    { name: 'Mike', age: 32, favoriteFoods: ['Sushi', 'Salad'] },
    { name: 'Alice', age: 28, favoriteFoods: ['Steak', 'Fries'] }
  ];
  
  // Create multiple records
  Person.create(arrayOfPeople, (err, data) => {
    if (err) return console.error('Error creating multiple records:', err);
    console.log('Multiple people created:', data);
  });

  // Find all people with the name "John Doe"
Person.find({ name: 'John Doe' }, (err, people) => {
    if (err) return console.error('Error finding people:', err);
    console.log('Found people:', people);
  });

  // Find one person with a certain favorite food
const food = 'Burrito';
Person.findOne({ favoriteFoods: food }, (err, person) => {
  if (err) return console.error('Error finding person:', err);
  console.log('Found person:', person);
});

// Find a person by their _id (replace with a valid ID)
const personId = '60d5ec49f1d5a3408c6ec09e';
Person.findById(personId, (err, person) => {
  if (err) return console.error('Error finding person by ID:', err);
  console.log('Found person by ID:', person);
});

// Find a person by ID and update their favoriteFoods
Person.findById(personId, (err, person) => {
    if (err) return console.error('Error finding person:', err);
    
    // Add "hamburger" to the person's favoriteFoods
    person.favoriteFoods.push('hamburger');
    
    // Save the updated person
    person.save((err, updatedPerson) => {
      if (err) return console.error('Error saving updated person:', err);
      console.log('Updated person:', updatedPerson);
    });
  });

  // Update a person's age by name
const personName = 'Mary';
Person.findOneAndUpdate({ name: personName }, { age: 20 }, { new: true }, (err, updatedPerson) => {
  if (err) return console.error('Error updating person age:', err);
  console.log('Updated person:', updatedPerson);
});

// Delete one person by their _id
Person.findByIdAndRemove(personId, (err, removedPerson) => {
    if (err) return console.error('Error removing person:', err);
    console.log('Removed person:', removedPerson);
  });

  // Delete all people named "Mary"
Person.remove({ name: 'Mary' }, (err, result) => {
    if (err) return console.error('Error deleting people:', err);
    console.log('Result of delete operation:', result);
  });

  // Find people who like burritos, sort them by name, limit to 2, and select fields
Person.find({ favoriteFoods: 'Burrito' })
.sort({ name: 1 })
.limit(2)
.select({ age: 0 }) // Exclude age from the results
.exec((err, data) => {
  if (err) return console.error('Error finding people:', err);
  console.log('People who like burritos:', data);
});
