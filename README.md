# 9_bio-auth_vejledning


# 7_camera_vejledning

## Slutresultat
#### Denne opgave kan ikke køres på simulator på, da du skal bruge dine BIOS!

## App.js
1. Start med at oprette et nyt projekt.
2. Installér følgende dependencies:
   1. ``expo install @react-native-community/masked-view @react-navigation/bottom-tabs @react-navigation/native @react-navigation/stack expo-app-loading expo-facebook expo-google-app-auth expo-local-authentication react-native-safe-area-context react-native-safe-area-view react-native-screens react-native-reanimated react-native-vector-icons``


## AppStruktur
1. Opret nu følgende 3 komponenter BioScreen, FacebookScreen og GoogleScreen
2. `Husk også at komponentnavn skal være ens med filnavnet`


## App.js
1. Opret en tab-bar navigator med de tilhøre 3 komponenter 
2. Ved Facebook Screen tab'en, lav et shorthand if statement, hvis man er på IOS ``Se styling øvelsen``
3. 


## BioScreen.js ( big boi)
1. **useState** | Lav en bio, setBio state, som har et initialState objekt med attributterne `hasBiometricHardware: false, hasBiometricData: false, isRequestingBiometricLogin: false, isLoggedInBiometic:false`
2. **useEffect** | Opret nu en useEffect function som har et tomt dependency
   1. Deri lav med et shorthand if statement, som tjekker på bio.hasBiometricHardware || bio.hasBiometricData er false (hint: !)
   2. efter && kald nu på en funktion kaldt checkBiometricAvailability(), som derefter oprettes med hint 1
3. **Return** | Wrap nu return statement med et if else statement, som tjekker på om bio.isLoggedInBiometic er false. Heri opret også et return statement i else'et
   1. IF | Her i skal du lave et View, som har noget styling, og deri lav et tekst element som skriver at du kan logge ind
   2. IF | Lig nu hint 2 ind, og deri første shorthand if statementen lav en knap som kalder på en funktion kaldt requestBiometricLogin, og en titel Biometric login , og lav dertil hørende en funktion. I det andet if statement lav en knap med en funktion kaldt cancelBiometricLogin og med titlen Cancel
   3. Else | lav et View som har dertil har Tekst som viser du er logget ind, og lav en knap som kalder en funktion kaldt logout og lav dertil funktionen
4. **requestBiometricLogin** | Først lav requestBiometric til en async funktion 
   1. Opret et try catch, hvor vi i catchen console.log'er fejlen ud
   2. Først ændre bio.isRequestingBiometricLogin til true hint (`setState({...state,attributeNavn:true})`)
   3. Prøv nu at logge ind hvor du får et respone fra LocalAuthentication.authenticateAsync. Få her inspiration fra https://docs.expo.dev/versions/latest/sdk/local-authentication/#localauthenticationauthenticateasyncoptions 
      1. Her vil vi gerne i promptMessage spørge om log in med FacebID/TouchId
      2. Fallback, skal være use your passcode
   4. Derefter skal du lave et if else statement, hvor du i IF statementen kigger på response.success er true
      1. IF | sæt isLoggedInBiometic til true
      2. else | `Alert.alert('Failure');`
   5. Lav nu et nyt if statement med response.error, som i statement vil alerte med response.error
5. **cancelBiometricLogin** Her skal du kalde på LocalAuthentication.cancelAuthenticate();
6. **logout**, her skal du sætte isLoggedInBiometic til false
7. **Reference**: https://docs.expo.dev/versions/latest/sdk/local-authentication/ 


## GoogleScreen.js


## Ekstra opgave


## Hints
### skabalon 1
`const checkBiometricAvailability = async () => {
const hasBiometricHardware = await LocalAuthentication.hasHardwareAsync();
const hasBiometricData = await LocalAuthentication.isEnrolledAsync();
setBio({...bio,hasBiometricHardware,hasBiometricData})
};`

### skabalon 2
`{bio.hasBiometricData && bio.hasBiometricHardware && ({/*Knapper*/})}`
`{bio.isRequestingBiometricLogin && Platform.OS === 'android' && ({/*Knapper*/})}`

## Referencer
https://docs.expo.dev/versions/latest/sdk/camera/

https://reactnavigation.org/docs/stack-navigator/
https://reactnative.dev/docs/statusbar
