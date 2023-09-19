---
title: Getting started with React Native
---

You can use Instant in React Native projects too! Below is an example using Expo. Open up your terminal and do the following:

```shell
npx create-expo-app instant-rn-demo
cd instant-rn-demo
yarn add @instantdb/react-native
yarn start
```

Now open up `src/App.js` in your favorite editor and replace the entirity of the file with the following code.

```javascript
import { init, useQuery, transact, tx } from "@instantdb/react-native";
import { View, Text, Linking, Button, StyleSheet } from "react-native";

const APP_ID = "REPLACE ME";

init({
  appId: APP_ID,
  websocketURI: "wss://api.instantdb.com/runtime/session",
});

function App() {
  const { isLoading, error, data } = useQuery({ colors: {} });

  if (isLoading) {
    return (
      <View style={{ padding: 10 }}>
        <Text>
          If you are seeing this you likely need to replace{' '}
          <Text style={{ fontWeight: 'bold' }}>APP_ID</Text> on line 4
        </Text>
        <Text>{"\n"}</Text>
        <Text>
          You can get your APP_ID by{' '}
          <Text
            style={{ color: 'blue' }}
            onPress={() => Linking.openURL('https://instantdb.com/dash')}>
            logging into your Instant dashboard
          </Text>
          . After replacing the id you may need to reload the page.
        </Text>
      </View>
    )
  )
  if (error) return <Text>Error: {error.message}</Text>;

  return <Main data={data} />;
}

const selectId = "4d39508b-9ee2-48a3-b70d-8192d9c5a059";

function Main({ data }) {

  const { colors } = data;
  const { color } = colors[0] || { color: "grey" };

  return (
    <View style={[styles.container, { backgroundColor: color }]}>
      <View style={styles.spaceY4}>
        <Text style={styles.header}>Hi! pick your favorite color</Text>
        <View style={styles.spaceX4}>
          {["green", "blue", "purple"].map((c) => {
            return (
              <Button
                title={c}
                onPress={() => {
                  transact(tx.colors[selectId].update({ color: c }));
                }}
                key={c}
              />
            );
          })}
        </View>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
  },
  spaceY4: {
    marginVertical: 16,
  },
  spaceX4: {
    flexDirection: "row",
    justifyContent: "space-between",
    marginHorizontal: 16,
  },
  header: {
    fontSize: 24,
    fontWeight: "bold",
    marginBottom: 16,
  },
});

export default App;
```

Huzzah 🎉 You've got your first React Native Instant app running! Check out the [**Explore**](/docs/init) section to learn more about how to use Instant :)