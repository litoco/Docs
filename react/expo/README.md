# Question and Answers
1. How to initialize react native application?
    <details>
      <summary>View Answer</summary>
      There is only one command to create the application:
      <ul>
        <li> Go to the folder where you want to create your application </li>
        <li> run <code>sudo npx create-expo-app testing_project</code> to create a project with name "testing_project" </li>
      </ul>
    </details>
2. How to start the project on emulator?
    <details>
      <summary>View Answer</summary>
      To start the react native project on any of android/ios emulators run <code>npx expo start</code> and then press <code>i</code>
    </details>
3. How to delete project?
    <details>
      <summary>View Answer</summary>
      Run command <code>sudo rm -rf testing_project</code> to delete 'testing_project' project
    </details>
4. How to debug the application running on physical device in chrome??

    <details>
      <summary>View Answer</summary>
      There is only one command to create the application:
      <ol>
        <li> go to project folder on the local machine </li>
        <li> run <code>npm start</code> </li>
        <li> scan qr code on physical device using expo go </li>
        <li> shake the physical device to bring up developers menu </li>
        <li> from the developers menu select <b>Open JS Debugger</b> </li>
        <li> go to the chrome tab that opens on local machine after step <b>"v"</b> </li>
        <li> from the chrome tab go to <b>Sources</b> window </li>
        <li> Click pause on exception button present on the top right hand side of <b>Sources</b> window </li>
        <li> Check the checkbox that appears below the button when you perform step 8 </li>
        <li> Thats all, you can now execute your app in debug mode </li>
        <li> The <b> watch pane </b> present on the right of the <b> Sources </b> window, keeps track of the variables and their value that we provide as input (by clicking the <b>"+"</b> button) </li>
        <li> To stop remote debugging, kill the chrome app that opened in debugging </li>
      </ol>
    </details>
5. How to make some content of `<text>` tag bold in react native?
    <details>
      <summary>View Answer</summary>
   
      By nesting `<text>` tags. Eg:
      ```
      
        const MyText = () => {
          return (
            &lt;Text&gt;
              This is a sentence with
              &lt;Text style={{ fontWeight: 'bold' }}>bold&lt;/Text&gt;
              text.
            &lt;/Text&gt;
          );
        };
      ```
      
    </details>
    
6. How to make prevent screen content from spilling over to the phone screen's notch?
    <details>
      <summary>View Answer</summary>
      
      By importing and using <code>&lt;SafeAreaView&gt;</code> tag we can achieve this functionality. This functionality is only available in iOS not in android.
   
    </details>
7. How to show image on the screen?
    <details>
      <summary>View Answer</summary>
      
      By importing and using <code>&lt;Image&gt;</code> tag we can achieve this functionality.</br>
      <code>&lt;Image source={require('./assets/favicon.png')} /&gt;</code> will render image from the location specified.</br>
      <code>&lt;Image source={{ height: 300, width: 200, uri: "https://picsum.photos/200/300" }} /&gt;</code> will render image from internet.
   
    </details>
8. How to handle clicks?
    <details>
      <summary>View Answer</summary>
      
      On the text component we can handle clicks using the onPress event but on image component this isn't possible so we need to use "touchableComponents". One of such is **`TouchableWithoutFeedback`**, **`TouchableOpacity`**, **`TouchableHighlight`**
   
    </details>
9. Why do we have style in separate variables?
    <details>
      <summary>View Answer</summary>
      
      Having styles in separate variable has 2 purposes: \
       i) If there is some error in the styles the inline ones don't show error but the errors are visible in the separated style concept.\
       ii) Make the code clean and reusable
      
    </details>
10. How to add alternative of `<SafeAreaView>` in android?
    <details>
      <summary>View Answer</summary>
      
      We can use padding for this purpose. For that we will first import **Platform, StatusBar** then use them as
      ```
      paddingTop: Platform.OS === "android" ? StatusBar.currentHeight : 0
      ```
      
    </details>
11. How to detect orientation changes?
    <details>
      <summary>View Answer</summary>

      Use react-native-community/hooks library. Download it and then

      ```
      import {
        useDimensions,
        useDeviceOrientation
      } from "@react-native-community/hooks"
      
      const {landscape} = useDeviceOrientation() {/* extract landscape object's value from object returned by useDeviceOrientation function */} 
      
      height: landscape ? "100%" : "30%" {/* if the orientation is landscape mode make the view fill the whole screen's height and if it is in portrait orientation make the height to be 30% of the screen */}
      ```

      
    </details>
12. What is a **FlexBox**?
    <details>
      <summary>View Answer</summary>

      FlexBox is a layout used to design complex views along the primary axis. It is similar to (linear layout view + weight) property of android. 
      For example:
      ```
      <View style={{backgroundColor: "#FF9933", flex: 2}}/> 
      <View style={{backgroundColor: "#FFFFFF", flex: 3}}/>
      <View style={{backgroundColor: "#138808", flex: 2}}/>
      ```
      
    </details>
13. What are some of the flex properties?
    <details>
      <summary>View Answer</summary>

      * We can use view's `flexDirection: row/column` property to align items horizontally or vertically. If the view is horizontal then x axis is the primary axis and y axis is the secondary axis. Similarly if view is vertical y axis is the primary axis and x axis is the secondary axis.\
      * If we want to align views on the primary axis we use `justifyContent` property else if we want to align views on the secondary axis we use `alignItems` property\
      * `alignSelf` is used to align view with respect to its parent along the secondary axis\
      * `alignItem` aligns views of each primary axis(like row) to the center of the screen along the secondary axis\
      * If you want to align the whole view to the center of the screen we can use `alignContent`. `alignContent` only work in `flexWrap: "wrap"`. If there is no wrapping `alignContent` has no effect\
      * `flexBasis` works same as width/length depending on the primary axis\
      * `flexGrow` fills the entire remaining primary axis, works as `flex: 1`\
      * `flexShrink` shrinks the view to make room for all the views to be visible on the screen, works same as `flex: -1`
      
    </details>
14. How to access object values inside react native components?
    <details>
      <summary>View Answer</summary>

      To access value inside a react native component simple import the file(containing details that we need) into the type script file, then access it using the object notation.
      Eg: to access `name` object from `products.ts` file into `index.tsx` file
      ```
      import Products from '../../../assets/data/products';
      
      const product = Products[1];
      
      export default function TabOneScreen() {
        return (
          <View style={styles.container}>
            <Text style={styles.title}>{product.name}</Text>
          </View>
        );
      }
      
      const styles = StyleSheet.create({
        container: {
          backgroundColor: "white",
          padding: 10,
          borderRadius: 20
        },
        title: {
          fontSize: 18,
          marginVertical: 10,
          fontWeight: '600',
        }
      });
      ```

    </details>
15. How to extract component and call it in the main UI view?
    <details>
      <summary>View Answer</summary>

      To extract component we can use lambda function like `ProductListItem` lambda function shown below:
      ```
      const ProductListItem = ({product}) => {
        return (
          <View style={styles.container}>
            <Image source={{uri: product.image}} style={styles.image}/>
            <Text style={styles.title}>{product.name}</Text>
            <Text style={styles.price}>${product.price}</Text>
          </View>
        );
      }
      
      export default function TabOneScreen() {
        return (
          <View>
          <ProductListItem product={Products[1]}/>
          <ProductListItem product={Products[2]}/>
        </View>
        );
      }
      ```
      
      We can also send this lambda function to a `.tsx` file, export it and then use it in the main `.tsx` file.
      Eg:
      
      **ProductListItem.tsx**
      ```
      const ProductListItem = ({product}) => {
        return (
          <View style={styles.container}>
            <Image source={{uri: product.image}} style={styles.image}/>
            <Text style={styles.title}>{product.name}</Text>
            <Text style={styles.price}>${product.price}</Text>
          </View>
        );
      };
      
      export default ProductListItem;
      
      const styles = StyleSheet.create({
        container: {
          backgroundColor: "white",
          padding: 10,
          borderRadius: 20
        },
        image: {
          width: "100%",
          aspectRatio: 1
        },
        title: {
          fontSize: 18,
          marginVertical: 10,
          fontWeight: '600',
        },
        price: {
          color: Colors.light.tint,
          fontWeight: 'bold'
        },
        separator: {
          marginVertical: 30,
          height: 1,
          width: '80%',
        },
      });
      ```
      
      **Main.tsx**
      ```
      import { View } from 'react-native';
      import Products from '../../../assets/data/products';
      import ProductListItem from '../../components/ProductListItem';
      
      
      export default function TabOneScreen() {
        return (
          <View>
          <ProductListItem product={Products[1]}/>
          <ProductListItem product={Products[2]}/>
        </View>
        );
      }
      ```
      
    </details>

16. How to make a type safe component?
    <details>
      <summary>View Answer</summary>
      
      Type safe components creation involves the following steps:
      1. Breakdown the component in smaller components and then build on top of that. The smaller component should have primitive data types, then export these components and use them into immediate parent component such that ultimately you get your main parent component. Eg:

      **types.tsx**
      ```
      export type Product = {
        id: number;
        image: string | null;
        name: string;
        price: number;
      };
      ```
      2. Use the component in the main `.tsx` file. Create a wrapper type and include the component as its property. Eg:
      ```
      import { Product } from '@/types';

      export const defaultPizzaImage = "https://notjustdev-dummy.s3.us-east-2.amazonaws.com/food/default.png";

      type ProductListItemProps = {
        product: Product;
      }

      const ProductListItem = ({ product }: ProductListItemProps) => {
        return (
          <View style={styles.container}>
            <Image source={{ uri: product.image || defaultPizzaImage }} style={styles.image}/>
            <Text style={styles.title}>{product.name}</Text>
            <Text style={styles.price}>${product.price}</Text>
          </View>
        );
      }

      export default ProductListItem;
      ```
      Here we have used `ProductListItemProps` as wrapper and `product` is a property of this wrapper. Later we used this property in the lambda function below.
      **NOTE:**
      The uri part can be null as defined in the `Product` definition, hence we used a default value `defaultPizzaImage` to manage null urls and improving UX 

    </details>

17. How to implement infinitely scroll list?
    <details>
      <summary>View Answer</summary>
      
      To implement **infinite scrolling** we use `<FlatList>`. It takes two inputs:
      1. data list
      2. How to render each item of data list
      Eg:
      ```
      <FlatList 
      data = {products}
      renderItem = {({item}) => <ProductListItem product={item}}
      numColumns={2}
      contentContainerStyle={{gap:10, padding: 16}}
      columnWrapperStyle={{gap: 10}}/>
      ```
      `numColumns` divides the screen in two equal spaced columns\
      `contentContainerStyle` is used to style the row. Eg: here we provide the gap between rows to be `10` and gap between the row and screen to be `16`\
      `columnWrapperStyle` is used to style each column item. Eg: here we provide the gap between column and column boundary to be `10`

    </details>
