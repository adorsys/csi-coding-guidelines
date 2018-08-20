## Biometric Login

### A sample implementation

```swift
import LocalAuthentication

class BiometricLogin {
    private let context: LAContext
    private let authenticator: Authenticator

    init(context: LAContext = .init(),
         authenticator: Authenticator = .init()) {
        self.context = context
        self.authenticator = authenticator
    }

    var isBiometricLoginAvailable: Bool {
        return authenticator.canAuthenticateViaToken
            && context.canEvaluatePolicy(.deviceOwnerAuthenticationWithBiometrics, error: nil)
    }

    var loginButtonTitle: String {
        let biometryType = [
            LABiometryType.faceID: "Face ID",
            LABiometryType.touchID: "Touch ID"
        ][context.biometryType]

        return String(format: "Sign in with %@", biometryType ?? "")
    }

    func performBiometricLogin(completion: @escaping (_ success: Bool) -> Void) {
        // This reason is only displayed in the TouchID alert.
        // For FaceID, see NSFaceIDUsageDescription in Info.plist
        context.evaluatePolicy(.deviceOwnerAuthenticationWithBiometrics,
                               localizedReason: .touchIDReason) { success, _ in
            DispatchQueue.main.async {
                completion(success)
            }
        }
    }
}
```

This class exposes 3 features:
- A way to check if biometric login should be used
    - This depends on device settings and token availability
- A title for a login button deciding what kind of authentication is used
- A function to perform the actual authentication

It can be used like this:

```swift
class LoginViewController: UIViewController {

    @IBOutlet weak var primaryButton: UIButton!
    let biometricLogin = BiometricLogin()

    override func viewDidLoad() {
        super.viewDidLoad()
        if biometricLogin.isBiometricLoginAvailable {
            primaryButton.setTitle(biometricLogin.loginButtonTitle, for: .normal)
        } else {
            primaryButton.setTitle("Mit Mail einloggen", for: .normal)
        }
    }

    @IBAction func didTapPrimaryButton() {
        if biometricLogin.isBiometricLoginAvailable {
            biometricLogin.performBiometricLogin { success in
                // perform login here if biometry check was successful
            }
        } else {
            loginWithMailAddress()
        }
    }
}
```

### General pointers

- The same `LAContext` instance must be used for `canEvaluatePolicy` and `evaluatePolicy`
- A `localizedReason` is only required for TouchID
- Make sure to include an `NSFaceIDUsageDescription` in your Info.plist. Your app *will crash* on iPhone X if you don't.
- Check out the Human Interface Guidelines and see if they make sense for your use case

### Further Reading

- [Human Interface Guidelines on biometric login][hig]
- [Blog post about TouchID and Face ID][blogpost]

[hig]: https://developer.apple.com/design/human-interface-guidelines/ios/user-interaction/authentication/
[blogpost]: http://michael-brown.net/2018/touch-id-and-face-id-on-ios/
