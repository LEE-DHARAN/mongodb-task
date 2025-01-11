1.users Collection:


db.users.insertMany([
  {
    username: "student1",
    email: "student1@gmail.com",
    fullName: "student one",
    dateJoined: new ISODate("2023-09-15"),
    role: "student",
    tasks: “Not Submitted”,
    attendance: “Absent”,
    codekata: 20
  },
  {
    username: "student2",
    email: "student2@yahoo.com",
    fullName: "student two",
    dateJoined: new ISODate("2023-09-15"),
    role: "student",
    tasks: 8,
    attendance: “present”,
    codekata: 20
  },

  {
    username: "mentor1",
    email: "mentor1@gmail.com",
    fullName: "Mentor One",
    dateJoined: new ISODate("2020-01-01"),
    role: "mentor",
    mentees: 16
  }
  {
    username: "mentor2",
    email: "mentor2@gmail.com",
    fullName: "Mentor One",
    dateJoined: new ISODate("2020-01-01"),
    role: "mentor",
    mentees: 14
  }

]);


2.codekata Collection:


{
  "_id": ObjectId("676bf1ce32e3d0519350adc1"),
  "user_id": ObjectId("5f9d4b3b7f8d9c7a5e125f89"),
  "solved_problems": [
    {"problem1", "solved_on": ISODate("2020-10-10")},
    {"problem2", "solved_on": ISODate("2020-10-15")}
  ]
}


3.attendance Collection:


db.attendance.insertMany([
{
  "_id": ObjectId("676bf2b032e3d0519350adc4"),
  "user_id": ObjectId("5f9d4b3b7f8d9c7a5e125f89"),
  
"date": ISODate("2020-10-18"),
  "status": "Present" 
},
{
  "_id": ObjectId("676bf2b032e3d0519350adc5"),
  "user_id": ObjectId("5f9d4b3b7f8d9c7a5e125f90"),
"date": ISODate("2020-10-18"),
  "status": "absent" 
}
  ]);


4.topics Collection:


db.topics.insertMany([
{
  "_id": ObjectId("676bf3a932e3d0519350adc8"),
  "topic_name": "MongoDB",
  "scheduled_date": ISODate("2020-10-05"),
  "duration": 90 // minutes
},
{
  "_id": ObjectId("676bf3a932e3d0519350adc9"),
  "topic_name": "SQL",
  "scheduled_date": ISODate("2020-11-01"),
  "duration": 90 // minutes
}
]);


5.tasks Collection:


db.tasks.insertMany([
{
  "_id": ObjectId("676bf3e032e3d0519350adcc"),
  "task_name": "MongoDB Assignment",
  "assigned_on": ISODate("2020-10-10"),
  "due_date": ISODate("2020-10-15"),
  "status": “Submitted”,
  "user_id": ObjectId("5f9d4b3b7f8d9c7a5e125f89")
},
{
  "_id": ObjectId("676bf3e032e3d0519350adcd"),
  "task_name": "SQL Assignment",
  "assigned_on": ISODate("2020-10-10"),
  "due_date": ISODate("2020-10-15"),
  "status": "Not Submitted",
  "user_id": ObjectId("5f9d4b3b7f8d9c7a5e125f90")
}
]);


6.company_drives Collection:


db.company_drives.insertMany([
{
  "_id": ObjectId("676bf46132e3d0519350add0"),
  "company_name": "GUVI",
  "drive_date": ISODate("2020-10-20"),
  "user_ids": [ObjectId("676bf0d032e3d0519350adbf"), ObjectId("676bf0d032e3d0519350adc0")] 
},
{
  "_id": ObjectId("676bf46132e3d0519350add1"),
  "company_name": "ZEN",
  "drive_date": ISODate("2020-11-01"),
  "user_ids": [ObjectId("676bf0d032e3d0519350adbf"), ObjectId("676bf0d032e3d0519350adc0")] 
}]);


7.mentors Collection:


{
  "_id": ObjectId("676bf4b732e3d0519350add4"),
  "name": "Dharan",
  "mentee_count": 20,
  "mentee_ids": [ObjectId("5f9d4b3b7f8d9c7a5e125f89"), ObjectId("5f9d4b3b7f8d9c7a5e125f90")]
}


Queries:


1. Find all the topics and tasks which were taught in the month of October.

db.topics.find({
  "scheduled_date": {
    $gte: ISODate("2020-10-01T00:00:00Z"),
    $lt: ISODate("2020-11-01T00:00:00Z")
  }
});

db.tasks.find({
  "assigned_on": {
    $gte: ISODate("2020-10-01T00:00:00Z"),
    $lt: ISODate("2020-11-01T00:00:00Z")
  }
});

2. Find all the company drives which appeared between 15 Oct 2020 and 31 Oct 2020.
db.company_drives.find({

  "drive_date": {
    $gte: ISODate("2020-10-15T00:00:00Z"),
    $lte: ISODate("2020-10-31T23:59:59Z")
  }
});


3. Find all the company drives and students who appeared for the placement.

db.company_drives.find({
  "drive_date": {
    $gte: ISODate("2020-10-01T00:00:00Z"),
    $lt: ISODate("2020-11-01T00:00:00Z")
  }
});

db.users.find({
  "_id": { $in: [ObjectId("676bf0d032e3d0519350adbf"), ObjectId("676bf0d032e3d0519350adc0")] }
});

4. Find the number of problems solved by the user in Codekata.

db.codekata.find({
  "user_id": ObjectId("676bf1ce32e3d0519350adc1")
});

5. Find all the mentors with more than 15 mentees.

db.mentors.find({

  "mentee_count": { $gt: 15 }
});

6. Find the number of users who are absent and task is not submitted between 15 Oct 2020 and 31 Oct 2020.

db.attendance.find({
  "status": "Absent",
  "date": { $gte: ISODate("2020-10-15T00:00:00Z"), $lte: ISODate("2020-10-31T23:59:59Z") }
});

db.tasks.find({
  "status": "Not Submitted",
  "assigned_on": { $gte: ISODate("2020-10-15T00:00:00Z"), $lte: ISODate("2020-10-31T23:59:59Z") }
});
