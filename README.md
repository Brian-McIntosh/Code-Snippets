# Code-Snippets
A collection of miscellaneous code snippets for Alerts, Keyboard Binding, etc.

## Showing an Alert
```swift
    // Create alert
    let alert = UIAlertController(title: "Add Person", message: "What is their name?", preferredStyle: .alert)
    alert.addTextField()

    // Configure button handler - background thread
    let submitButton = UIAlertAction(title: "Add", style: .default) { (action) in

        // Get the textfield for the alert
        let textfield = alert.textFields![0]

        // Create a Person object
        // Person is a subclass of NSManagedObject which allows us to save to Core Data
        let newPerson = Person(context: self.context)
        newPerson.name = textfield.text

        // Save the data
        // try! self.context.save() // error w/o try! should use do/catch
        do {
            try self.context.save()
        }
        catch {

        }

        // Re-fetch the data
        // this function handles the context, fetchRequest, and reloading of tableview on main thread
        self.fetchPeople()
    }

    // Add button
    alert.addAction(submitButton)

    // Show alert
    self.present(alert, animated: true, completion: nil)
```

## Add Swipe and Delete Action
```swift
    func tableView(_ tableView: UITableView, trailingSwipeActionsConfigurationForRowAt indexPath: IndexPath) -> UISwipeActionsConfiguration? {
        
        // Create swipe action
        let action = UIContextualAction(style: .destructive, title: "Delete") { (action, view, completionHandler) in
            
            // Which person to remove?
            let personToRemove = self.items![indexPath.row]
            
            // Delete the person from the context
            self.context.delete(personToRemove)
            
            // Save the data
            try! self.context.save() // DO/CATCH
            
            // Re-fetch the data
            self.fetchPeople()
        }
        
        // Return swipe actions
        return UISwipeActionsConfiguration(actions: [action])
        
    }
```
