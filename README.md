# REALM-CRUD

```swift
// MARK: READ
var toDoItems: Results<Task>? {
    get {
        guard let realm = LocalDatabaseManager.realm else {
            return nil
        }
        return realm.objects(Task.self)
    }
}
```

```swift
// MARK: UPDATE
guard let realm = LocalDatabaseManager.realm else {
    return 
}

do {
    try realm.write {
        toDoItem.isComplete = true
    }
} catch let error as NSError {
    print(error.localizedDescription)
    return
}
```

```swift
// MARK: DELETE
guard let realm = LocalDatabaseManager.realm else {
    return 
}
do {
    try realm.write {
        realm.delete(self.toDoItems![indexPath.row])
    }
} catch let error as NSError {
    print(error.localizedDescription)
    return
}
```

```swift
// MARK: CREATE
// MARK: REALM WRITE
guard let realm = LocalDatabaseManager.realm else {
    reportError(title: "Error", message: "A new task could not be created")
    return
}

let nextTaskId = (realm.objects(Task.self).max(ofProperty: "id") as Int? ?? 0) + 1
let newTask = Task()
newTask.id = nextTaskId
newTask.name = taskName
newTask.details = taskDetails
newTask.completionDate = completionDate as NSDate
newTask.isComplete = false

do {
    try realm.write {
        realm.add(newTask)
    }
} catch let error as NSError {
    print(error.localizedDescription)
    reportError(title: "Error", message: "A new task could not be created")
}
```
