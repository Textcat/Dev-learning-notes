# A simple plist manager written in swift
#Programming/Snippets/Swift 
```swift
internal class PlistDicManager{
    static func readPlistObject(with key:String) -> Any?{
        let path = NSHomeDirectory()+"/snkcooker/userconfig.plist"
        guard let dict = NSMutableDictionary(contentsOfFile: path) else {return nil}
        guard let object = dict[key] else {return nil}
        
        return object
    }
    
    
    static func writePlistObject(object:Any,forKey:String) {
        let path = NSHomeDirectory()+"/snkcooker/userconfig.plist"
        let dict = NSMutableDictionary(contentsOfFile: path)

        dict?.setValue(object, forKey: forKey)
        dict?.write(toFile: path, atomically: true)
    }
    
    
    static func updatePlistObject(forKey key:String, process:(_ object:Any) -> Any?) {
        if let object = self.readPlistObject(with: key) {
            if let newObject = process(object) {
                self.writePlistObject(object: newObject, forKey: key)
            }
        }
    }
    
    
    static func checkExist() {
        let path = NSHomeDirectory()+"/snkcooker/userconfig.plist"
        let fileManager = FileManager.default
        if !fileManager.fileExists(atPath: path) {
            
            do {
                try fileManager.createDirectory(atPath: NSHomeDirectory()+"/snkcooker", withIntermediateDirectories: true, attributes:nil)
                
                let srcPath = Bundle.main.path(forResource: "userconfig", ofType: "plist")
                
                do {
                    //Copy the project plist file to the documents directory.
                    try fileManager.copyItem(atPath: srcPath!, toPath: path)
                } catch {
                    print("File copy error!")
                }
            }catch {
                print("create directory fail")
            }
        }
    }
}
```