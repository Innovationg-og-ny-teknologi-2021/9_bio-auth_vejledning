# 9_bio-auth_vejledning

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
1. Opret en tab-bar navigator med de ovennævnte 3 komponenter 
2. Ved Facebook Screen tab'en, lav et shorthand if statement, hvis man er på IOS ``Se styling øvelsen``
   1. Det er på grund af Facebook login på Android virker ikke uden en key som i skal hente fra facebook


## BioScreen.js ( big boi)
1. **useState** | Lav en bio, setBio state, som har et initialState objekt med attributterne `hasBiometricHardware: false, hasBiometricData: false, isRequestingBiometricLogin: false, isLoggedInBiometic:false`
2. **useEffect** | Opret nu en useEffect function som har en tom dependency
   1. Deri lav med et shorthand if statement, som tjekker på bio.hasBiometricHardware || bio.hasBiometricData er false (hint: !)
   2. efter && kald nu på en funktion kaldt checkBiometricAvailability(), som derefter oprettes med hint 1
3. **Return** | Wrap nu return statement med et if-else statement, som tjekker på om bio.isLoggedInBiometic er false. Heri oprettes også et return statement i din else blok
   1. IF | Her i skal du lave et View, som har noget styling, og deri lav et tekst element som skriver at du kan logge ind
   2. IF | Lig nu hint 2 ind, og deri første shorthand if statementen lav en knap som kalder på en funktion kaldt requestBiometricLogin, og en titel Biometric login , og lav en dertilhørende funktion. I det andet if statement laves en knap med en funktion, som kaldes cancelBiometricLogin og med titlen Cancel
   3. Else | lav et View som fremviser tekst, der angiver at du er logget ind. Dernæst laves en som kalder en funktion på logout. I forlængelse af dettes konstrueres funktionen
4. **requestBiometricLogin** | Først lav requestBiometric til en async funktion 
   1. Opret et try catch, hvor vi i catchen console.log'er fejlen.
   2. try | Først ændre bio.isRequestingBiometricLogin til true; HINT (`setState({...state,attributeNavn:true})`)
   3. try | Prøv nu at logge ind hvor du får et response fra LocalAuthentication.authenticateAsync. Få her inspiration fra https://docs.expo.dev/versions/latest/sdk/local-authentication/#localauthenticationauthenticateasyncoptions 
      1. Her vil vi gerne i promptMessage spørge om log in med FacebID/TouchId
      2. Fallback, skal være use your passcode
   4. Derefter skal du lave et if-else statement, hvor du i IF statementet kigger på response.success er true
      1. IF | sæt isLoggedInBiometic til true
      2. else | `Alert.alert('Failure');`
   5. Lav nu et nyt if statement med response.error, som i statement vil lave en alert med response.error
5. **cancelBiometricLogin** Her skal du kalde på LocalAuthentication.cancelAuthenticate();
6. **logout**, her skal du sætte isLoggedInBiometic til false
7. **Reference**: https://docs.expo.dev/versions/latest/sdk/local-authentication/ 


## GoogleScreen.js
1. **useState** | lav nu two useStates: en kaldt googleUser med initial value på null, og en anden kaldt googleToken med en tom string
2. **config** opret en googleConfig med hint 3
3. **Return** | Wrap nu et return statement med et if-else statement, som tjekker på om googleUser er false. Heri oprettes også et return statement i else-blokken
   1. IF | Her i skal du lave et View, som har noget styling, og deri lav et tekst element som skriver at du kan logge ind
   2. IF | Lav en knap som kalder på en funktion kaldt handleGoogleLogin og lav dertil en funktion
   3. Else | lav et View som angiver at du er logget ind, og lav en knap som kalder en funktion kaldt handleLogOut og lav dertil funktionen
4. **handleGoogleLogin** - opret som en async funktion
   1. Opret et try catch, hvor vi i catchen console.log'er fejlen 
   2. try | skriv nu følgende `const {type, accessToken, user} = await Google.logInAsync(googleConfig);`
   3. lav nu et if , else if og else. I if tjek om type er succes, hvor du sætter tilstanden(setstate) for googleUser med user, og googleToken med accesToken. Dernæst tjekkes der i else-if løkken om; type === cancel, hvorefter der oprettes en alert medcancelled. til sidst i else laves en Alert med beskeden "ooops noget gik galt"
5. **handleLogOut**
   1. lav ligeledes en try catch
   2. try | kald `await Google.logOutAsync({accessToken: googleToken, ...googleConfig});` og derefter sæt begge states til deres standard værdier

## FacebookScreen.js
1. **useState** | lav nu en useState: en kaldt facebookUser med initial value på null.
2. Lig nu følgende ind efter useState:`const FacebookID = '2633284050312146'; const FacebookAppId = 'App_login';`
3. **return** | lav et View, som har et Text og Button i sig. Button skal kalde på en funktion kaldt logIn ( lav den ), og Text skal indeholde en shorthand if-else statement, som kigger på om man er logget ind - hint ∆
   1. Wrap nu Button med et shorthand if statement, så knappen kun bliver vist hvis man ikke er logget ind
4. **logIn** | lav først til en async funktion
   1. Opret et try catch, hvor vi i catchen console.log'er fejlen ud
   2. try | hint ß


## Ekstra opgave


## Hints
### hint 1
`const checkBiometricAvailability = async () => {
const hasBiometricHardware = await LocalAuthentication.hasHardwareAsync();
const hasBiometricData = await LocalAuthentication.isEnrolledAsync();
setBio({...bio,hasBiometricHardware,hasBiometricData})
};`

### hint 2
`{bio.hasBiometricData && bio.hasBiometricHardware && ({/*Knapper*/})}`
`{bio.isRequestingBiometricLogin && Platform.OS === 'android' && ({/*Knapper*/})}`

### hint 4
`const googleConfig = {
/*Android/IOS ID behøver ikke at blive skiftet */
iosClientId: '603386649315-vp4revvrcgrcjme51ebuhbkbspl048l9.apps.googleusercontent.com',
androidClientId: '603386649315-9rbv8vmv2vvftetfbvlrbufcps1fajqf.apps.googleusercontent.com',
iosStandaloneAppClientId: '<IOS_CLIENT_ID>',
androidStandaloneAppClientId: '<ANDROID_CLIENT_ID>'
}`

### hint ∆
{ facebookUser ? `Hej ${facebookUser.name}` : "Login into your facebook account"}

### hint ß
`
await Facebook.initializeAsync({
   appId: FacebookID,
   appName:FacebookAppId
});
const { type, token, permissions} = await Facebook.logInWithReadPermissionsAsync({
   permissions: ['public_profile','name'],
});
console.log(permissions,"permisson")
if (type === 'success') {
   // Get the user's name using Facebook's Graph API
   const response = await fetch("https://graph.facebook.com/me/?fields=id,name&access_token=${token}");
   setFacebookUser(response.json());
} else {
    // type === 'cancel'
}
`

## Referencer
https://docs.expo.dev/versions/latest/sdk/camera/

https://reactnavigation.org/docs/stack-navigator/
https://reactnative.dev/docs/statusbar
