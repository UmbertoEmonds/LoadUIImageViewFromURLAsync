# LoadUIImageViewFromURLAsync

Load image asynchronously to UIImageView with picture URL and fade animation !

Use this method ```loadAsync(url:URL)``` with any UIImageView. Add this class to your project:

```
import Foundation
import UIKit

extension UIImageView {
    
    func loadAsync(url:URL){
        
        UIView.animate(withDuration: 1.0, animations: {
            self.alpha = 0.0
            self.alpha = 1.0
        })
        
        downloadImage(url: url)
        
    }
    
    func getDataFromUrl(url: URL, completion: @escaping (_ data: Data?, _  response: URLResponse?, _ error: Error?) -> Void) {
        URLSession.shared.dataTask(with: url) {
            (data, response, error) in
            completion(data, response, error)
            }.resume()
    }
    
    func downloadImage(url: URL) {
        
        getDataFromUrl(url: url) { (data, response, error)  in
            guard let data = data, error == nil else { return }
            print(response?.suggestedFilename ?? url.lastPathComponent)
            
            DispatchQueue.main.async() { () -> Void in
                self.image = UIImage(data: data)
            }
            
        }
        
    }
    
}
