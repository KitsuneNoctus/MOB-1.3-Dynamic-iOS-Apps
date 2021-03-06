# Alamofire + Firebase

## Minute-by-Minute

| **Elapsed** | **Time**  | **Activity**              |
| ----------- | --------- | ------------------------- |
| 0:00        | 0:05      | Objectives                |
| 0:05        | 0:05      | Initial Exercise          |
| 0:10        | 0:05      | Alamofire                 |
| 0:15        | 0:25      | In Class Activity I       |
| 0:40        | 0:10      | BREAK                     |
| 0:50        | 0:05      | Firebase                  |
| 0:55        | 0:50      | In Class Activity II      |
| 1:45        | 0:05      | Wrap Up                   |
| TOTAL       | 1:50      |                           |

## Why you should know this

There are many people in the Swift community that are always looking for ways to make writing apps easier and in a more standardized way. Thanks to them we have many open source libraries we can use and that we can rely on, simply because they have thousands of experienced collaborators. Alamofire is one of them, and is a great alternative to manage network requests. We'll see how, now that we have an understanding of URLSession, it's easier to understand what the library is doing. Another project that we'll use today as our backend is Firebase by Google. Firebase has many products for mobile and web development, the one we'll focus on is Firebase Database which will store and handle all our data in real time.

## Class Learning Objectives/Competencies (5 min)

By the end of this class, you should be able to...
1. Make network requests using Alamofire.
1. Set up a project on Firebase Database.
1. Read, write and delete data from Firebase Database.

## Initial Exercise (5 min)

### Swift podcasts recommendations:
- [Swift over coffee](https://itunes.apple.com/us/podcast/swift-over-coffee/id1435076502?mt=2) by Paul Hudson and Sean Allen (also on Spotify)<br>
- [Swift community podcast](https://www.swiftcommunitypodcast.org)<br>
- [Swift by Sundell](https://www.swiftbysundell.com/podcast)<br>
- [Swift Unwrapped](https://spec.fm/podcasts/swift-unwrapped)<br>
- [Fireside Swift](https://www.firesideswift.com)

## Alamofire (5 min)

Alamofire is a networking library for iOS, written in Swift. It's a wrapper around Apple's Foundation networking suite. It simplifies handling requests and responses, serializing JSON, authentication, among other things.

Methods available:

- Alamofire.upload: Upload files with multipart, stream, file or data methods.
- Alamofire.download: Download files or resume a download already in progress.
- Alamofire.request: Requests not associated with file transfers.

Something to note is that we don't need to instantiate a class to use Alamofire's methods because they are global within the library.

## In Class Activity I (25 min)

1. Using the pre-made starter app [Lesson10](https://github.com/VanderDev1/Lesson10.git), copy the following code in your ViewController. This is a network request that looks like the type of requests we've used so far.

<!-- Create a new project copy the following code in your ViewController. This is a network request that looks like the type of requests we've used so far.
-->

```Swift
        let endpoint: String = "https://jsonplaceholder.typicode.com/todos/1"
        guard let url = URL(string: endpoint) else {
            print("error creating the URL")
            return
        }
        let urlRequest = URLRequest(url: url)
        // creating the URLSession
        let session = URLSession(configuration: .default)
        // creating the data task
        let task = session.dataTask(with: urlRequest) {(data, response, error) in
            // check for any errors
            guard error == nil else {
                print("error calling the endpoint")
                print(error!)
                return
            }
            // validate data
            guard let responseData = data else {
                print("error receiving data")
                return
            }
            // parse the result as JSON
            do {
                guard let todo = try JSONSerialization.jsonObject(with: responseData, options: [])
                    as? [String: Any] else {
                        print("error converting data to JSON")
                        return
                }
                // if everything is fine, we get a JSON object. We want to print the title of the todo object.
                guard let title = todo["title"] as? String else {
                    print("couldn't find title")
                    return
                }
                print("The title is: " + title)
            } catch  {
                print("error converting data to JSON")
                return
            }
        }
        task.resume()
```
Analyze all the elements of the request, identify where you create the URLRequest, URLSession, dataTask, how to handle the response, data and error.

We are going to write the same request using Alamofire.

1. Go to [Alamofire](https://github.com/Alamofire/Alamofire)'s website and follow the instructions to include the library in your project. You will need to create a podfile and install the library via Cocoapods.

1. Now go back to your ViewController and import Alamofire
1. To make a simple request in Alamofire you will use the following code:
```Swift
Alamofire.request(endpoint).responseJSON { response in
    // handle response
}
// the default HTTP method is GET but we can specify it as well.
Alamofire.request(endpoint, method: .get).responseJSON { response in
  // handle response
}
```
This is how we would set up and send a request.<br>
Is that simple. What differences can you see on the way we are making the request?<br><br>
So what is `.responseJSON`? It's the way in which Alamofire calls `JSONSerialization` on our behalf. So we only worry if we're getting the JSON object in the expected type we specify.
<br><br>Now we have to handle the response.<br><br>
1. Check for an error
```Swift
guard response.result.error == nil ...
```
1. If there's none check if there's a JSON result.
```Swift
guard let json = response.result.value as? ...
```
1. If we can transform the JSON then we print out the title of the todo object.
```Swift
guard let todoTitle = json["title"] as? ...
```
Try to complete it on your own. Then validate with the final result:
```Swift
let endpoint: String = "https://jsonplaceholder.typicode.com/todos/1"
        Alamofire.request(endpoint).responseJSON { response in
                guard response.result.error == nil else {
                    print(response.result.error!)
                    return
                }
                guard let json = response.result.value as? [String: Any] else {
                    print("Error: \(response.result.error)")
                    return
                }
                guard let title = json["title"] as? String else {
                    print("Could not find title")
                    return
                }
                print("The title is: " + title)
        }
```
1. Now create another Alamofire request of type POST. <br>
Endpoint: https://jsonplaceholder.typicode.com/todos <br>
Parameters: ["title": "Super Cool Post", "completed": 0, "userId": 8]
1. If you have time left, ty a DELETE request to this endpoint: https://jsonplaceholder.typicode.com/todos/1 and print a message if the request was successful.

**Q:** When is it a good idea to use Alamofire over our own network implementation?<br>
**Q:** What are the pros and cons of using it?

## Firebase
Firebase is an app development platform that provides developers many tools to build high quality apps that can scale easily.

It has products to store data, handle notifications, do A/B testing, analytics, authentication and more. The ones we'll learn how to use today are the RealTime Database and Storage. These tools will enable us to read and write to a database.


## In Class Activity II
- [Loaner app + Firebase](assets/FirebaseGuide.md) activity.

## Wrap Up (5 min)

- Complete today's activities.
- Keep working on your project.

## Additional Resources
[Slides](https://docs.google.com/presentation/d/19Zmcl_UNufLoN_buWqD1es31rw6JHa4boFu2XlFoOF8/edit?usp=sharing)<br>
[Alamofire](https://github.com/Alamofire/Alamofire)<br>
[JSON placeholder](https://jsonplaceholder.typicode.com)<br>
[Alamofire walkthrough](https://grokswift.com/rest-with-alamofire-swiftyjson/)<br>
[Firebase](https://firebase.google.com/docs/ios/setup)
