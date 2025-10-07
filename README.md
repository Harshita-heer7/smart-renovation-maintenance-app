import React, { useState, useEffect } from "react";
import {
  View,
  Text,
  Button,
  FlatList,
  ActivityIndicator,
  StyleSheet,
  SafeAreaView,
} from "react-native";

// Backend API ka base URL
const BACKEND_BASE = "http://192.168.0.106:5000";


export default function App() {
  const [providers, setProviders] = useState([]);
  const [loading, setLoading] = useState(false);
  const [error, setError] = useState("");

  // Providers fetch karne ka function
  const fetchProviders = async () => {
  try {
    setLoading(true);
    setError("");
    let response = await fetch(`${BACKEND_BASE}/api/providers`);
    let data = await response.json();
    setProviders(data);
  } catch (err) {
    console.error("Fetch error:", err);
    setError("Failed to load providers");
  } finally {
    setLoading(false);
  }
};


  return (
    <SafeAreaView style={styles.container}>
      <Text style={styles.title}>Smart Renovation & Maintenance Planner 🚀</Text>

      <Button title="Load Providers" onPress={fetchProviders} />

      {loading && <ActivityIndicator size="large" color="#0000ff" />}
      {error ? <Text style={styles.error}>{error}</Text> : null}

      <FlatList
        data={providers}
        keyExtractor={(item) => item.provider_id.toString()}
        renderItem={({ item }) => (
          <View style={styles.card}>
            <Text style={styles.name}>{item.name}</Text>
            <Text>Service: {item.service_type}</Text>
            <Text>Cost per day: ₹{item.cost_per_day}</Text>
            <Text>Rating: ⭐ {item.rating}</Text>
            <Text>City: {item.city}</Text>
          </View>
        )}
      />
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, padding: 20, backgroundColor: "#fff" },
  title: { fontSize: 20, fontWeight: "bold", marginBottom: 10 },
  error: { color: "red", marginTop: 10 },
  card: {
    padding: 15,
    marginVertical: 8,
    backgroundColor: "#f0f0f0",
    borderRadius: 8,
  },
  name: { fontSize: 16, fontWeight: "bold" },
});

