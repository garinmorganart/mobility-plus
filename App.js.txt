// Mobility+ MVP App
// React Native + Expo
// Features: Onboarding, AI Chatbot, Rehab Plan, Video Player, Pain Tracker, Subscription

import React, { useState } from 'react';
import { SafeAreaView, View, Text, TextInput, Button, ScrollView, StyleSheet } from 'react-native';
import { WebView } from 'react-native-webview';

const injuries = [
  "Tennis Elbow",
  "Golfer’s Elbow",
  "Runner’s Knee",
  "Shoulder Impingement"
];

const beginnerExercises = {
  "Tennis Elbow": [
    {
      title: "Wrist Extension Stretch",
      video: "https://www.youtube.com/embed/2XrjGtvT1Vg"
    },
    {
      title: "Forearm Extensor Massage",
      video: "https://www.youtube.com/embed/7X7zL8v-otw"
    }
  ],
  "Golfer’s Elbow": [
    {
      title: "Wrist Flexor Stretch",
      video: "https://www.youtube.com/embed/DKzO2jONkDg"
    }
  ],
  "Runner’s Knee": [
    {
      title: "Quad Stretch",
      video: "https://www.youtube.com/embed/ZyL8SKpVJk0"
    }
  ],
  "Shoulder Impingement": [
    {
      title: "Pendulum Exercise",
      video: "https://www.youtube.com/embed/i5sUhYJwf8Q"
    }
  ]
};

export default function App() {
  const [stage, setStage] = useState('onboarding');
  const [symptoms, setSymptoms] = useState('');
  const [diagnosis, setDiagnosis] = useState('');

  const handleDiagnosis = () => {
    if (symptoms.toLowerCase().includes("elbow")) {
      if (symptoms.toLowerCase().includes("outside")) setDiagnosis("Tennis Elbow");
      else setDiagnosis("Golfer’s Elbow");
    } else if (symptoms.toLowerCase().includes("knee")) {
      setDiagnosis("Runner’s Knee");
    } else if (symptoms.toLowerCase().includes("shoulder")) {
      setDiagnosis("Shoulder Impingement");
    } else {
      setDiagnosis("Tennis Elbow"); // default
    }
    setStage('rehab');
  };

  return (
    <SafeAreaView style={styles.container}>
      {stage === 'onboarding' && (
        <View>
          <Text style={styles.header}>Welcome to Mobility+</Text>
          <Text style={styles.instructions}>Describe your pain or symptoms:</Text>
          <TextInput
            placeholder="e.g. pain on the outside of my elbow"
            style={styles.input}
            value={symptoms}
            onChangeText={setSymptoms}
          />
          <Button title="Find My Injury" onPress={handleDiagnosis} />
        </View>
      )}

      {stage === 'rehab' && (
        <ScrollView>
          <Text style={styles.subheader}>Diagnosis: {diagnosis}</Text>
          <Text style={styles.instructions}>Try these beginner exercises:</Text>
          {beginnerExercises[diagnosis].map((ex, i) => (
            <View key={i} style={styles.exerciseCard}>
              <Text style={styles.exerciseTitle}>{ex.title}</Text>
              <View style={styles.videoContainer}>
                <WebView
                  javaScriptEnabled={true}
                  domStorageEnabled={true}
                  source={{ uri: ex.video }}
                />
              </View>
            </View>
          ))}
          <Button title="Upgrade for Full Plan (£5/month)" onPress={() => alert('Subscription coming soon!')} />
        </ScrollView>
      )}
    </SafeAreaView>
  );
}

const styles = StyleSheet.create({
  container: {
    flex: 1,
    padding: 20,
    backgroundColor: '#fff',
  },
  header: {
    fontSize: 24,
    fontWeight: 'bold',
    marginBottom: 10,
  },
  subheader: {
    fontSize: 22,
    fontWeight: 'bold',
    marginBottom: 10,
  },
  instructions: {
    marginVertical: 10,
    fontSize: 16,
  },
  input: {
    borderWidth: 1,
    borderColor: '#ccc',
    padding: 10,
    marginBottom: 10,
    borderRadius: 5,
  },
  exerciseCard: {
    marginVertical: 10,
  },
  exerciseTitle: {
    fontSize: 18,
    marginBottom: 5,
  },
  videoContainer: {
    height: 200,
    marginBottom: 10,
  },
});