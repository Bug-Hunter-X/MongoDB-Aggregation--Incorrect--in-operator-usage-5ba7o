# MongoDB Aggregation: Incorrect $in operator usage
This repository demonstrates a common error when using the `$in` operator in MongoDB aggregation pipelines. Specifically, the error involves using a mix of `ObjectId` and string values within the `$in` array. This can lead to unexpected results because the `ObjectId` and strings are not directly comparable.

## Bug
The bug lies in the following code snippet:
```javascript
db.collection.aggregate([
  {
    $match: {
      _id: { $in: [ObjectId('651765176517651765176517'), 'stringID']}
    }
  }
]);
```
The `$in` operator expects values of the same type.  Mixing `ObjectId` and string types results in a failure to match documents correctly.

## Solution
The solution is to ensure that all values within the `$in` array are of the same type.  Here are two possible solutions:
1. **Convert all IDs to strings:** 
   This approach is simpler if the application doesn't heavily rely on ObjectId functionality and only needs string representation of IDs.
2. **Convert all IDs to ObjectIds:**  This approach is preferred if working with ObjectIds directly is required.  The `ObjectId()` constructor in the MongoDB driver will take care of the conversion.

## How to run
1. Clone the repo.
2. Set up a MongoDB instance.
3. Insert a few documents into the collection with a mix of `ObjectId` and string IDs.
4. Run the `bug.js` script to reproduce the error.
5. Run the `bugSolution.js` script to see the corrected aggregation pipeline.
