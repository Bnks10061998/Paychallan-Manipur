import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { List } from "react-native-paper";

const PayChallanScreen = ({ navigation }) => {
  const [expanded, setExpanded] = useState(null);
  const [vehicleNumber, setVehicleNumber] = useState("");

  const toggleExpand = (index) => {
    setExpanded(expanded === index ? null : index);
  };

  return (
    <ScrollView style={{ flex: 1, backgroundColor: "#f8f8f8", paddingHorizontal: 15 }}>
      {/* Header */}
      <TouchableOpacity onPress={() => navigation.goBack()} style={{ flexDirection: "row", alignItems: "center", marginVertical: 10 }}>
        <Ionicons name="arrow-back" size={24} color="black" />
        <Text style={{ fontSize: 18, fontWeight: "bold", marginLeft: 10 }}>Pay Challan</Text>
      </TouchableOpacity>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, elevation: 2, marginBottom: 15 }}>
        <Text style={{ fontSize: 16, fontWeight: "bold", marginBottom: 8 }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8 }}>Enter your vehicle number and check your challans</Text>
        <View style={{ flexDirection: "row", alignItems: "center", backgroundColor: "#f0f0f0", borderRadius: 8, paddingHorizontal: 10 }}>
          <TextInput
            placeholder="Enter Your Vehicle Number"
            value={vehicleNumber}
            onChangeText={setVehicleNumber}
            style={{ flex: 1, height: 40 }}
          />
          <TouchableOpacity style={{ backgroundColor: "#28a745", paddingVertical: 10, paddingHorizontal: 15, borderRadius: 6 }}>
            <Text style={{ color: "white", fontWeight: "bold" }}>Check</Text>
          </TouchableOpacity>
        </View>
      </View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, elevation: 2, marginBottom: 15 }}>
        <Text style={{ fontSize: 16, fontWeight: "bold", marginBottom: 8 }}>How it works?</Text>
        <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
          <View style={{ alignItems: "center", flex: 1 }}>
            <Ionicons name="car-outline" size={30} color="#007bff" />
            <Text style={{ fontSize: 14 }}>Step 1</Text>
            <Text style={{ fontSize: 12, color: "#666" }}>Enter vehicle number</Text>
          </View>
          <Text>➡️</Text>
          <View style={{ alignItems: "center", flex: 1 }}>
            <Ionicons name="document-text-outline" size={30} color="#007bff" />
            <Text style={{ fontSize: 14 }}>Step 2</Text>
            <Text style={{ fontSize: 12, color: "#666" }}>Check your challans</Text>
          </View>
          <Text>➡️</Text>
          <View style={{ alignItems: "center", flex: 1 }}>
            <Ionicons name="cash-outline" size={30} color="#007bff" />
            <Text style={{ fontSize: 14 }}>Step 3</Text>
            <Text style={{ fontSize: 12, color: "#666" }}>Make payment</Text>
          </View>
        </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, elevation: 2, marginBottom: 15 }}>
        <Text style={{ fontSize: 16, fontWeight: "bold", marginBottom: 8 }}>Traffic Fines</Text>
        <TouchableOpacity style={{ backgroundColor: "#f0f0f0", padding: 12, borderRadius: 8 }}>
          <Text>Select Your State</Text>
        </TouchableOpacity>
      </View>

      {/* Invite Friends Section */}
      <View style={{ backgroundColor: "#28a745", padding: 15, borderRadius: 10, elevation: 2, marginBottom: 15 }}>
        <Text style={{ fontSize: 16, fontWeight: "bold", color: "white" }}>Invite your friends to make hassle-free Challan Payments</Text>
        <Text style={{ color: "white", fontSize: 12, marginVertical: 5 }}>Pay using UPI, Debit & Credit Cards</Text>
        <TouchableOpacity style={{ backgroundColor: "white", padding: 8, borderRadius: 8, alignItems: "center" }}>
          <Text style={{ color: "#28a745", fontSize: 14, fontWeight: "bold" }}>Invite Now</Text>
        </TouchableOpacity>
      </View>

      {/* FAQs Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, elevation: 2, marginBottom: 15 }}>
        <Text style={{ fontSize: 16, fontWeight: "bold", marginBottom: 8 }}>FAQs</Text>
        {[
          { question: "What is a traffic challan?", answer: "A traffic challan is a penalty issued by authorities for violating traffic rules." },
          { question: "Can I contest or dispute a traffic challan?", answer: "Yes, you can dispute a challan via the respective state’s transport website." },
          { question: "How can I avoid future traffic challans?", answer: "Follow all traffic rules, obey speed limits, and avoid reckless driving." },
          { question: "How can I download or print a copy of my challan receipt?", answer: "You can download it from the official e-challan website after payment." },
        ].map((faq, index) => (
          <List.Accordion
            key={index}
            title={faq.question}
            expanded={expanded === index}
            onPress={() => toggleExpand(index)}
            style={{ backgroundColor: "transparent", borderBottomWidth: 1, borderColor: "#ddd" }}
            titleStyle={{ fontSize: 14, fontWeight: "bold" }}
          >
            <Text style={{ padding: 10, fontSize: 12 }}>{faq.answer}</Text>
          </List.Accordion>
        ))}
      </View>
    </ScrollView>
  );
};

export default PayChallanScreen;
