# REST API using Node.js, Express and Mongodb

  1. Install [Node](https://nodejs.org/en/download), [Mongo Compass](https://www.mongodb.com/try/download/compass), [Postman](https://www.postman.com/downloads/) and [VSCode](https://code.visualstudio.com/)
  2. Create **package.json** by running `npm init`
  3. Install dependencies `npm i express mongoose nodemon dotenv`
  4. Create instances and start listening to port
     ```bash
      const express = require('express');
      const mongoose = require('mongoose');
      const app = express();
      app.use(express.json());
      app.listen(3000, () => { console.log(`Server Started at ${3000}`)})
     ```
  5. Run `npm start` you will see message `Server Started at 3000`
  6. In the terminal you will see `Server Started at 3000`
  7. In **package.json**, under `scripts` add `"start": "nodemon index.js"`
  8. Create a [Mongo DB](https://account.mongodb.com/account/login) account
  9. Create a free shared cluster
  10. After creating a cluster, click `Connect`
  11. Copy the Database URL, create **.env** file in local project root add entry `DATABASE_URL = mongodb+srv://banu0:<password>@cluster0.wzwgl1u.mongodb.net/banu`
  12. Add your DB URL to MongoDB Compass and connect
  13. Connect to your DB
      ```bash
      require('dotenv').config();
      const mongoString = process.env.DATABASE_URL;
      mongoose.connect(mongoString);
      const database = mongoose.connection;
      database.on('error', (error) => { console.log(error) })
      database.once('connected', () => { console.log('Database Connected'); })
      ```
  14. In the terminal you will see `Database Connected`
  15. Create a folder **routes** and add a file **routes.js** into it
  16. Import this file in **index.js** `const routes = require('./routes/routes');`
  17. Add routes
      ```bash
      const app = express();
      app.use(express.json());
      const routes = require('./routes/routes');
      app.use('/api', routes);
      ```
  18. In **routes.js** Add endpoints in the format
      ```bash
      const express = require('express');
      const router = express.Router();
      const Model = require('../models/model');
      //Post Method
      router.post('/post', async (req, res) => { const data = new Model({ name: req.body.name, age: req.body.age })
      try {
        const dataToSave = await data.save();
        res.status(200).json(dataToSave)
      } catch (error) { res.status(400).json({message: error.message})}})

      // getAll Endpoint
      router.get('/getAll', async (req, res) => {
      try{
        const data = await Model.find();
        res.json(data)
      } catch(error){ res.status(500).json({message: error.message})}})

      //Get by ID Method
      router.get('/getOne/:id', async (req, res) => {
      try{
        const data = await Model.findById(req.params.id);
        res.json(data)
      } catch(error){ res.status(500).json({message: error.message})}})

      //Update by ID Method
      router.patch('/update/:id', async (req, res) => {
      try {
        const id = req.params.id;
        const updatedData = req.body;
        const options = { new: true };

        const result = await Model.findByIdAndUpdate(
            id, updatedData, options
        )
        res.send(result)
      } catch (error) { res.status(400).json({ message: error.message })}})
    
      //Delete by ID Method
      router.delete('/delete/:id', async (req, res) => {
      try {
        const id = req.params.id;
        const data = await Model.findByIdAndDelete(id)
        res.send(`Document with ${data.name} has been deleted..`)
      } catch (error) { res.status(400).json({ message: error.message })}})
    
      module.exports = router;
      ```
18. Now you can use Postman to use API and view the data in MongoDB Compass
<img width="850" alt="Screenshot 2023-10-20 at 6 06 25 PM" src="https://github.com/SurulirajBanu/rest_api_docker_node_mongo/assets/85181631/dfc81ee7-3b0d-400c-8cb1-a7c3a3f6cd1c">
<img width="852" alt="Screenshot 2023-10-20 at 6 07 51 PM" src="https://github.com/SurulirajBanu/rest_api_docker_node_mongo/assets/85181631/fcb3ab1a-a9d4-41a0-94ee-da863e4d91e2">
<img width="1435" alt="Screenshot 2023-10-20 at 6 08 07 PM" src="https://github.com/SurulirajBanu/rest_api_docker_node_mongo/assets/85181631/f6a19585-007b-46c3-879e-ae580fbb6d9e">
<img width="860" alt="Screenshot 2023-10-20 at 6 10 43 PM" src="https://github.com/SurulirajBanu/rest_api_docker_node_mongo/assets/85181631/f2837bea-1809-470b-91f0-bcdbfd489ac5">
<img width="854" alt="Screenshot 2023-10-20 at 6 11 55 PM" src="https://github.com/SurulirajBanu/rest_api_docker_node_mongo/assets/85181631/0047035c-fd7d-4c51-9043-6312ef60dfa1">
<img width="847" alt="Screenshot 2023-10-20 at 6 12 28 PM" src="https://github.com/SurulirajBanu/rest_api_docker_node_mongo/assets/85181631/99d60de7-cc58-4abe-833f-5978c4b40a30">




