**HEADS UP:**
<hr>

This question and answer section an mostly from the following youtube video:\
[Reference Video](https://www.youtube.com/watch?v=rIYzLhkG9TA)\
kindly go through it for complete reference

<hr>



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

18. What is expo router and how to implement it?
    <details>
      <summary>View Answer</summary>
      
      The **app** folder is where screens are located in expo. So to navigate to another page from current page we use `<Link>` component and provide a href with the link like `href='/product'`. This will find the `product.tsx` file in `app` directory and open it on navigation. 

      **NOTE:** 
      1. To navigating to another screen on click of a `<View>`, replace the `<View>` tag with `<Pressable>` tag since `<View>` tag doesn't support `onPress` event, but `<Pressable>` tag does.
      2. We can use `asChild` to the `<Link>` tag to allow react to render screen based on the styles of the children of `<Link>`
      3. To navigate to a specific `id` we can create a file with the name `[id].tsx` in the `app` folder. Now we can navigate to the specific `id` by providing it as value to the `href` property of `<Link>` tag. Eg: If we want to navigate to page `1`, `2`, `3` and so on we add it like `href='/1'`, `href='/2'`, `href='/3'` and so on. If we want the value of this `href` tag to be a variable we can use backticks like `` href=`/${product.id}` `` in `<Link>` tag
      4. To receive this `id` on the landing screen file we can use `const { id } = useLocalSearchParams();` and then use it's value as `{id}`.

    </details>

19. How to show a screen inside **bottom tab navigator**?
    <details>
      <summary>View Answer</summary>

      <h2>TL;DR</h2>
      
      **To init expo react navigation project run command `sudo npx create-expo-app@latest FoodOrdering -t` and choose the navigation option from the menu using the arrow keys**
      ><h3>Premise</h3>
      >
      >We have a screen called `[id].tsx` in `app` directory. We want this screen to be visible inside our tabs screen.
      >
      >For now our `[id].tsx` resides in `app` directory and `(tabs)` directory resides inside `app` directory hence `[id].tsx` is shown outside of our **bottom tab navigator** in a separate screen.
      >
      >The task is to make `[id].tsx` inside our **bottom tab navigator** screen
      >
      >So we have 2 screens inside the `Menu` tab and one screen (for now) in the `Orders` tab

      The two screens (namely `Menu List` screen and `Pizza Details` screen) are needed to be inside of `Menu` tab. Since we want to navigate to another screen from inside of tab navigator, this is a `nested navigation` usecase.\
      The `app/(tabs)/` directory holds screen files that are to be shown inside **bottom tab navigator**.\
    For our usecase we want to open `Pizza Details` screen after we click on an item of `Menu List` screen. So we need to put these two screens into one directory `app/(tabs)/menu`. To be specific we create `menu` directory inside `app/(tabs)/` and put `[id].tsx`, `index.tsx` into it.

      Even after moving the 2 screens `[id].tsx` and `index.tsx` into `app/(tabs)/menu` directory, we find that these 2 screens are shown as separate tabs in the tab navigator. To fix this we need `_layout.tsx` inside our `app/(tabs)/menu`. The content of this layout file is as following:
      ```
      import { Stack } from "expo-router";

      export default function MenuStack() {
          return <Stack />;
      }
      ```
      **NOTE:** 
      * Notice that the default screen that gets shown on to the screen is present in `index.tsx` file of that directory. Eg: Inside `app/(tabs)/` directory the screen will shown according to `app/(tabs)/index.tsx` and the screen shown in `app/(tabs)/menu/` will be `app/(tabs)/menu/index.tsx`
      </br>
      Now that we have our screen working as expected we need to change the details in the tab navigator layout which is present in `app/(tabs)/_layout.tsx`. Change the `name="index"` to `name="menu"` inside the `<Tabs.Screen>` tag.

      If we refresh the screen, we see `This screen doesn't exist.` message. This is because there is no `app/(tabs)/index.tsx` file present in the tabs folder. So we will create `app/(tabs)/index.tsx` file and provide it the following content:
      ```
      import { Redirect } from 'expo-router';

      export default function TabIndex () {
        return <Redirect href={'/menu/'} />;
      };
      ```
      In the above code we are simply redirecting the screens content to contain the the content inside `app/(tabs)/menu/index.tsx` file.

      If we refresh the app, again we see `index` tab present in the **tab navigator**. To remove this, we simply tell the **tab navigator** to hide the `index` tab. This done by going to `app/(tabs)/_layout.tsx` and adding `<Tabs.Screen name="menu" options={{href: null}}>`. Here we are hiding the `index` tab since we have already redirected **tab navigator** from `app/(tabs)/index.tsx` to `app/(tabs)/menu/index.tsx`

      If we try to go to the `Pizza Details` page from the `Menu` page. It doesn't work because the folder structures have changed to accommodate these change we change `/(tabs)${product.id}` to `/(tabs)/menu/${product.id}` in `app/components/ProductListItem.tsx` file to make it work.

      Now we are left with only one problem of double header. One header is coming from `app/(tabs)/_layout.tsx`, so we can hide that as well by adding `headerShown: false` property to the `options` property of `<Tabs.Screen name="menu">` tag.

      One last thing left is to change the title of `Menu` screen to `Menu` and `Pizza Details` screen to `Pizza: [id]`. To do that we go to `app/(tabs)/menu/_layout.tsx` file and replace it content with:
      ```
      import { Stack } from "expo-router";

      export default function MenuStack() {
          return <Stack>
              <Stack.Screen name="index" options={{title: "Menu", headerTitleAlign: 'center'}}/>
          </Stack>;
      }
      ```

      and `app/(tabs)/menu/[id].tsx` files content to:
      ```
      import { View, Text } from 'react-native'
      import React from 'react'
      import { Stack, useLocalSearchParams } from 'expo-router';

      const ProductDetailsScreen = () => {
          const { id } = useLocalSearchParams(); 

          return (
              <View>
                  <Stack.Screen options={{title: 'Pizza:' + id, headerTitleAlign: 'center'}} />
                  <Text>Product Details Screen: {id}</Text>
              </View>
          );
      };

      export default ProductDetailsScreen;
      ```
      In the above code the `title` option provides title to the page.
    
      **AND FINALLY, WE ARE DONE**

    </details>


20. What is state and how do we implement it?
    <details>
      <summary>View Answer</summary>
      
      State is a react object that contains data about the component. Unlike variables they re-render the component every time their value changes.\
      To implement state we import the state object from react native and use it inside the component:

      ```
      import React, { useState } from 'react';
      ...
      ...

      const sizes = ["S", "M", "L", "XL"];

      const ProductScreen = () => {
        ...
        const [selectedSize, setSelectedSize] = useState('M'); /* setSelectedSize is used as a setter method */
        ...
        
        return (
          <View style={styles.sizes}>
                {sizes.map((size) => (
                    <Pressable 
                    onPress= {() => {
                        setSelectedSize(size)
                    }}
                    style={[styles.size, {backgroundColor: selectedSize === size ? 'gainsboro': 'white'}]} 
                    key={size}>
                        <Text style={[styles.sizeText, {color: selectedSize === size ? 'black': 'grey'}]}>{size}</Text>
                    </Pressable>
                ))}
            </View>
        )


      }

      const styles = StyleSheet.create({
        sizes: {
            flexDirection: 'row',
            justifyContent: 'space-around',
            marginVertical: 10
        },
        size: {
            backgroundColor: 'gainsboro',
            width: 50,
            aspectRatio: 1,
            borderRadius: 25,
            justifyContent: 'center',
            alignItems: 'center'
        },
        sizeText: {
            fontSize: 20,
            fontWeight: '500'
        }
      });

      export default ProductScreen;
      ```
      
    </details>


21. What is modal and how do we implement it?
    <details>
      <summary>View Answer</summary>
      
      A modal is the react native component to display content above a view. To implement it we need to: create a modal page (lets say we name it to be `cart.tsx`) in the `app/` directory. Then we need to tell react native to treat it as a modal, we do that by specifying it in the `app/_layout.tsx` file as:
      ```
      function RootLayoutNav() {
        const colorScheme = useColorScheme();

        return (
          <ThemeProvider value={colorScheme === 'dark' ? DarkTheme : DefaultTheme}>
            <Stack>
              ...
              <Stack.Screen name="cart" options={{ presentation: 'modal' }} />
            </Stack>
          </ThemeProvider>
        );
      }
      ```

      After that we need to link to a event to open the page, like a **press event**. For example in this case we linked it to open when we click on cart icon on the menu bar. Following is the code for that we had the following code in `app/(tabs)/menu/_layout.tsx`:
      ```
      export default function MenuStack() {
          return <Stack
              screenOptions = {{
                  headerRight: () => (
                      <Link href="/cart" asChild>
                        <Pressable>
                          {({ pressed }) => (
                            <FontAwesome
                              name="shopping-cart"
                              size={25}
                              color={Colors.light.text}
                              style={{ marginRight: 15, opacity: pressed ? 0.5 : 1 }}
                            />
                          )}
                        </Pressable>
                      </Link>
                    )
              }}
          >
              ...
          </Stack>;
      }
      ```

    </details>

22. What is **`react context`** and how do we implement it?
    <details>
      <summary>View Answer</summary>
      
      If we pass the `state` which is generated in the parent component, to the children as `props` and the children don't consume the `props` instead they just pass it to another component that consumes it, this can lead to a problem called `Prop Drilling`. Even though it works it is hard to maintain, hence we should avoid it.

      In stead of `state` we can use `react context` in such cases. So `react context` is used to shared data between the different screens avoiding `Prop Drilling`. To implement following are the steps:
      1. Define a `context provider`. Here, we create it in `app/provider` directory
      2. Then we create context in that file 
        ```
        import { createContext } from 'react';
        import { CartItem, Product } from '@/types';
 
        type cartType = {
          items: CartItem[];
          addItem: (product: Product, size: CartItem['size']) => void;
        }

        export const CartContext = createContext<cartType>({
          items: [],
          addItem: () => {}, 
        });

        const CartProvider = ({ children }: PropsWithChildren) => {

          const [items, setItems] = useState<CartItem[]>([]);
          const addItem = (product:Product, size:CartItem['size']) => {
            console.log(product)
          };

          return (
            <CartContext.Provider  /* Provides the values to the other components */
            value = {{ /* The value that we want to provide context consumers */
                items: items,
                onAddItem: () => {}
              }}  
            > 
            { children } /* These children will have access to the values that we provide in this context provider */
            </CartContext.Provider> 
          );
        };

        export default CartProvider; /* we will wrap the screen that would share data between them using this context provider hence we exporting it */
        ```
      3. Once we have defined the `context provider`, next we need to wrap the screens that need the context provider. For that in here we go to the root _layout file and wrap the context provider to the whole app screen.
        **app/_layout.tsx:**
        ```
        import CartProvider from '@/providers/CartProvider';

        ...
        ...

        function RootLayoutNav() {
          const colorScheme = useColorScheme();

          return (
            <ThemeProvider value={colorScheme === 'dark' ? DarkTheme : DefaultTheme}>
              <CartProvider>
                <Stack>
                  <Stack.Screen name="(tabs)" options={{ headerShown: false }} />
                  <Stack.Screen name="cart" options={{ presentation: 'modal' }} />
                </Stack>
              </CartProvider>
            </ThemeProvider>
          );
        };

        ```
        The `<Stack>` component is a child of `<CartProvider>` component. This is the child that we are referring to as `children` in the 2nd step. So behind the scene `children` get replaced by `<Stack>` component at run time in the `<CartComponent>` component. And needless to say that all the items inside the `<Stack>` will have the values that are exported from  `<CartProvider value = {{...}}>` component which is a `contextProvider` component.

        4. Now we need to consume the values inside the children component. For consuming value we can use `ContextConsumer` or `useContext()`. We will use `useContext()` here. So to consume the value that we passed in `items array` in the `CartContext` we will modify **apps/cart.tsx** as following:
        ```
        import hook and cart context
        import { useContext } from 'react'; /* we use this hook to get values out from the context provider */
        import { CartContext } from '@/providers/CartProvider';

        const CartScreen = () => {
          const { items } = useContext(CartContext);

          return (
            ...
            ...
            console.log(items.length)
          );
        };
        ```

        We can also move the `useContext(CartContext)` to the context provider itself and later use it in wherever we want to consume this context provider. So in the **app/provider/CartProvider.tsx**
        ```
        ...
        ...
        export const useCart = () => useContext(CartContext);
        ```
        and use it in **app/cart.tsx**
        ```
        import { useCart } from '@/providers/CartProvider';
        ...
        ...
        const { items } = useCart(); /* instead of importing use context and CartContext */
        ```

        5. At last we need to update the value in `contextProviders` and see it in action. So in the **app/(tabs)/menus/[id].tsx** file, we will get the `contextProvider` and add data to it. This data will be propagated to `cart screen` using `contextProviders`. Following are the changes:
        ```
        ...
        import { useCart } from '@/providers/CartProvider';
        ...
        ...
        const ProductDetailsScreen = () => {
          ...
          const { addItem } = useCart();
          ...
          ...
          addItem(product, selectedSize);
        }
        ```




    </details>
