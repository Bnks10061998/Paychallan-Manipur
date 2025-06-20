import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView, Modal, FlatList,Image,StyleSheet,Share,StatusBar   } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { LinearGradient } from 'expo-linear-gradient';
import Collapsible from "react-native-collapsible";
import { NavigationIndependentTree } from '@react-navigation/native';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
import { MaterialCommunityIcons } from "@expo/vector-icons";
import Svg, { Rect, Circle, Text as SvgText,Path,Line } from "react-native-svg";



const Stack = createStackNavigator();


const states = [
  "Andaman and Nicobar Islands", "Andhra Pradesh", "Arunachal Pradesh", "Assam",
  "Bihar", "Chandigarh", "Delhi", "Goa", "Gujarat", "Haryana",
  "Jharkhand", "Karnataka", "Kerala", "Madhya Pradesh", "Maharashtra", "Manipur",
  "Meghalaya","Mizoram","Nagaland","Odisha","Punjab","Rajasthan","Sikkim","Tamil Nadu",
  "Telangana","Uttar Pradesh","West Bengal",
];
const challans = [
  {
    id: "1",
    vehicleNumber: "TN01B5230",
    dueDate: "13 Dec 2024",
    amount: "₹ 1000",
    status: "Unpaid",
    challanId: "TN123456789012345678",
  },
  {
    id: "2",
    vehicleNumber: "TN01B5230",
    dueDate: "30 Nov 2024",
    amount: "₹ 500",
    status: "Unpaid",
    challanId: "TN123456789012345678",
  },
];
const PayChallanScreen = ({ navigation }) => {
  const [vehicleNumber, setVehicleNumber] = useState("");
  const [modalVisible, setModalVisible] = useState(false);
  const [selectedState, setSelectedState] = useState("");
  const [errorMessage, setErrorMessage] = useState("");
  const [faqs, setFaqs] = useState([
    { question: "What is a traffic challan ?", answer: "A traffic challan is a fine or penalty imposed on individuals for violating traffic rules and regulations.", collapsed: true },
    { question: "Can I contest or dispute a traffic \nchallan ?", answer: "Yes, you can contest or dispute a traffic challan by following the procedures and deadlines set by your local traffic authorities.", collapsed: true },
    { question: "How can I avoid future traffic challans ?", answer: "1. Follow traffic rules and signs.\n2. Maintain speed limits.\n3. Use seatbelts and helmets.\n4. Avoid using mobile devices while driving.\n5. Maintain proper documentation (license, insurance).", collapsed: true },
    { question: "How can I download or print a copy of \nmy challan receipt ?", answer: "Click on the \"Download\" option to obtain a copy of your challan receipt.", collapsed: true },
  ]);
  const [referralCode] = useState("https://parkqwik.referrallink.one...");

  const toggleFAQ = (index) => {
    setFaqs(prevFaqs => prevFaqs.map((faq, i) => (i === index ? { ...faq, collapsed: !faq.collapsed } : faq)));
  };

  const shareReferral = async () => {
    try {
      await Share.share({ message: `Checkout ParkQwik App ${referralCode}` });
    } catch (error) {
      console.log("Error sharing referral code:", error);
    }
  };


  const validateInput = (text) => {
    setVehicleNumber(text);

    
     if (!/^[A-Za-z][A-Za-z0-9]{9}$/.test(text)) {
      setErrorMessage("Invalid format.");
    } 
    else {
      setErrorMessage(""); 
    }
  };

  const isValid = /^[A-Za-z][A-Za-z0-9]{9}$/.test(vehicleNumber);

  

  return (
    <ScrollView style={{ flex: 1, backgroundColor: "#FFFFFF", paddingHorizontal: 0 }}
    stickyHeaderIndices={[0]} >
      {/* Header */}
      <View 
  style={{  
    backgroundColor: "#1A9E75",
    height: 88, 
    width: "100%", 
    flexDirection: "row",
    alignItems: "center", 
    paddingHorizontal: 5,
    paddingTop: 25, 
  }}
>
  <TouchableOpacity onPress={() => navigation.goBack()} style={{ position: "absolute", left: 13 ,paddingTop:0}}>
    <Ionicons name="arrow-back" size={24} color="#FFFFFF" />
  </TouchableOpacity>
  <Text 
    style={{ 
      fontSize: 16, 
      fontWeight: "700", 
      color: "#FFFBFB", 
      fontFamily: 'Poppins',
      marginLeft: 50, 
    }}
  >
    Pay Challan
  </Text>
</View>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 15, marginTop: 17, elevation: 2, marginHorizontal: 20,paddingBottom: 20,paddingTop: 11 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 5,fontFamily: 'Poppins-Medium' }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8,lineHeight:20,fontFamily: 'Poppins-Regular'  }}>Enter your vehicle number and check {"\n"}your challans</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5, borderRadius: 8 }}>
    <TextInput
      placeholder="Enter Your Vehicle Number"
      value={vehicleNumber}
      onChangeText={validateInput}
      maxLength={10}
      placeholderTextColor="#1A9E75"
      style={{
        width: 190, 
        height: 40,
        backgroundColor: "#F0FFFA",
        borderWidth: 1,
        borderColor: isValid ? "#1A9E75" : "red",
        borderColor: "#1A9E75",
        borderRadius: 8,
        paddingHorizontal: 8,
        color: "#1A9E75",
        fontFamily: 'Poppins-Medium',
        fontSize: 14, 
      }}
    />

   

    <TouchableOpacity
      style={{
        backgroundColor: isValid ? "#1A9E75" : "#D3D3D3",
        paddingVertical: 10,
        paddingHorizontal: 20,
        borderRadius: 8,
        marginLeft: 2,
      }}
      onPress={() => navigation.navigate("ChallanDetailsScreen", { vehicleNumber })}
      disabled={!isValid}
    >
      <Text style={{ color: "white", fontWeight: "bold",fontSize: 14, }}>Check</Text>
    </TouchableOpacity>
  </View>

   {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5,alignSelf: "flex-start" }}>
            {errorMessage}
          </Text>
        )}
</View>

      
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8,marginTop: -5,fontFamily: "Poppins-Medium", }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        
        <View style={{  marginHorizontal: 4 }}>
          <View
            style={{
              backgroundColor: "#F0FFFA",
              borderRadius: 50,
              width: 65,
              height: 65,
              alignItems: "center",
              justifyContent: "center",
            }}
          >
<Svg width="46" height="45" viewBox="0 0 180 100">
    <Rect x="10" y="20" width="160" height="60" rx="10" fill="#1A9E75" />
    <Rect x="20" y="30" width="140" height="40" rx="6" fill="white" />
    <Circle cx="28" cy="36" r="4" fill="#1A9E75" />
    <Circle cx="152" cy="36" r="4" fill="#1A9E75" />
    <Circle cx="28" cy="62" r="4" fill="#1A9E75" />
    <Circle cx="152" cy="62" r="4" fill="#1A9E75" />
    
    <SvgText x="90" y="57" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "Medium", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14,fontFamily: "Poppins_Regular", }}>Enter your {"\n"} vehicle number</Text>
        </View>

      
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        
        <View style={{  marginHorizontal: 4 }}>
          <View
            style={{
              backgroundColor: "#F0FFFA",
              borderRadius: 50,
              width: 65,
              height: 65,
              alignItems: "center",
              justifyContent: "center",
            }}
          >
<Svg width="35" height="35" viewBox="0 0 64 64">
    
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />
   
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14,fontWeight: "Medium",fontFamily: "Poppins-Medium", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14,fontFamily: "Poppins-Regular", }}>Check Your {"\n"} Challans</Text>
        </View>

        
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        

        <View style={{  marginHorizontal: 4 }}>
          <View
            style={{
              backgroundColor: "#F0FFFA",
              borderRadius: 50,
              width: 65,
              height: 65,
              alignItems: "center",
              justifyContent: "center",
            }}
          >
            <Ionicons name="card-outline" size={30} color="#1A9E75" />
          </View>
          <Text style={{ fontSize: 14, fontFamily: "Poppins-Medium", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 3</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14,fontFamily: "Poppins-Regular", }}>Make {"\n"} Payment</Text>
        </View>
      </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, marginTop: 3, elevation: 2,width: "90%",marginHorizontal: 20 }}>
        <Text style={{ fontSize: 16,  marginBottom: 8,fontFamily: 'Poppins-SemiBold', }}>Traffic Fines</Text>{/* ,color:"#393939"*/}
        <Text style={{fontSize: 14,
                      color: "#AFAFAF",
                      marginBottom: 8,marginTop: -3,fontFamily: "Poppins-Regular",}}>Select your state to see traffic fines</Text>
        
      
      <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 10,
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#A0A0A0",
          borderWidth:1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          marginTop:4,
        }}
      >
        <Text style={{ color:"#AFAFAF", fontSize: 14 }}>
          {selectedState || "Select Your State"}
        </Text>
        <Ionicons name="caret-down-sharp" size={20} color="#27A47D" />
      </TouchableOpacity>

      
      <Modal
        visible={modalVisible}
        animationType="slide"
        transparent={true}
        onRequestClose={() => setModalVisible(false)}
      >
        <View style={{ flex: 1, justifyContent: "flex-end", backgroundColor: "rgba(0,0,0,0.3)" }}>
          <View style={{
            backgroundColor: "white",
            borderTopLeftRadius: 20,
            borderTopRightRadius: 20,
            padding: 20,
            maxHeight: "80%"
          }}>
            
            {/* Header with Close Button */}
            <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
              <Text style={{ fontSize: 18, fontWeight: "bold", color: "#333",marginBottom:14 }}>Choose State</Text>
              <TouchableOpacity onPress={() => setModalVisible(false)}>
                <Ionicons name="close-circle" size={24} color="green" />
              </TouchableOpacity>
            </View>

            {/* State List */}
            <FlatList
                data={states}
                keyExtractor={(item) => item}
                renderItem={({ item }) => (
                  <TouchableOpacity
                    style={{ flexDirection: "row", alignItems: "center", paddingVertical: 4 }}
                    onPress={() => {
                      setModalVisible(false);
                      navigation.navigate("StateChallanScreen", { stateName: item });
                    }}
                  >
                    <Ionicons name="radio-button-off" size={20} color="#1A9E75" style={{ marginRight: 10 }} />
                    <Text style={{ fontSize: 16, color: "#333" }}>{item}</Text>
                  </TouchableOpacity>
                )}
                showsVerticalScrollIndicator={false}
              />

          </View>
        </View>
      </Modal>
      </View>

      {/* Invite Friends Section */}
      <LinearGradient
                      colors={["#0BC189", "#27CD99"]}
                      style={{
                        padding: 10,
                        borderRadius: 15,
                        width: "91%", 
                        alignSelf: "center",
                        marginBottom: 5,
                        elevation: 5, 
                        shadowColor: "#000",
                        shadowOffset: { width: 0, height: 2 },
                        shadowOpacity: 0.2,
                        shadowRadius: 4,
                        backgroundColor: "#0BC189",
                        marginTop: 25, 
                      }}
                    >
  {/* Full Width Text */}
  <View style={{
    width: "100%", 
    padding:1,
    marginLeft:15,
    marginBottom: -10,
  }}>
    <Text style={{
      color: "#FBFF29",
      fontSize: 15,
      fontWeight:"Regular",
      fontFamily: "Arial",
      
    }}>
      Invite your friends to make hassle-free
    </Text>
  </View>

      
      <View style={{flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",}}>
        
        <View style={{flex: 1,
                      paddingRight: 10,}}>
          <Text style={{color: "white",
    fontSize: 18,
    fontFamily:"Arial",
    fontWeight: "Regular",
    marginBottom: 5,marginLeft:15}}>Challan Payments</Text>
          <Text style={{color: "white",
    fontSize: 10,
    fontFamily:"Arial",
    marginBottom: 10,
    fontWeight: "Regular",
    marginLeft:15}}>Pay using UPI, Debit & Credit Cards</Text>

        
          <TouchableOpacity style={{backgroundColor: "white",
                                    paddingVertical: 6,
                                    paddingHorizontal: 10,
                                    borderRadius: 15,
                                    alignItems: "center",
                                    alignSelf: "flex-start",marginLeft:15}} 
                                    onPress={shareReferral}>
          <Text style={{alignItems: "flex-end",fontSize:10,fontFamily: "Poppins",fontWeight:"semibold"}}>Invite Now</Text>
        </TouchableOpacity>
        </View>

        
        <View style={{width: 80,
                      height: 100,
                      alignItems: "center",
                      justifyContent: "center",
                      borderBottomRightRadius: 1, 
                      paddingTop: 28, 
                      
                       }}>
          <Image
            source={require("../../assets/images/mega1.jpg")} 
            style={{width: 120,
              height: 90,
              resizeMode: "contain",}}
          />
        </View>
      </View>
      
    </LinearGradient>
       
       <View style={{marginTop: 15,marginHorizontal:20 }}>
        <Text style={{fontSize: 16, fontWeight: "bold", color: "#333", marginBottom: 10,fontFamily: "Poppins-Medium",}}>FAQs</Text>
        {faqs.map((faq, index) => (
          <View key={index} style={{backgroundColor: "#FFFFFF", borderRadius: 10, marginBottom: 10, padding: 10, elevation: 2}}>
            <TouchableOpacity style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center", paddingVertical: 10 }} onPress={() => toggleFAQ(index)}>
              <Text style={{fontSize: 14, fontWeight: "Regular", color: "#393939", fontFamily: "Poppins",lineHeight:20}}>{faq.question}</Text>
              <Ionicons name={faq.collapsed ? "chevron-down" : "chevron-up"} size={20} color="#333" />
            </TouchableOpacity>
            <Collapsible collapsed={faq.collapsed}>
              <Text style={{fontSize: 14, color: "#A0A0A0", paddingTop: 4, paddingHorizontal: 5,lineHeight: 18,fontFamily: "Poppins-Regular"}}>{faq.answer}</Text>
            </Collapsible>
            
          </View>
        ))}
      </View>
    </ScrollView>
  );
};


const StateChallanScreen = ({ route, navigation }) => {
  const { stateName } = route.params;
  const [selectedState, setSelectedState] = useState("");
  const [modalVisible, setModalVisible] = useState(false);
  const [expandedIds, setExpandedIds] = useState([]);
  const finesData = [
    { id: "1", title: "Carrying Excess Luggage", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "2", title: "Driving Without a Number Plate", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "3", title: "Parking in No-parking Zone", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs.1500" },
    { id: "4", title: "Failure to Intimate Change of the Address of Vehicle Owner", amount: "₹ 500",details:"For the first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "25", title: "Driving a Vehicle \nRegistered in Another State for More Than 12 Months", amount: "₹ 500" },
    { id: "5", title: "Driving Without a Seatbelt", amount: "₹ 1000" },
    { id: "6", title: "Triple Riding on Two-vehicle", amount: "₹ 1000" },
    { id: "7", title: "Driving Without Helmet", amount: "₹ 1000" },
    { id: "8", title: "Over-speeding", amount: "₹ 1000",details:"For Light motor vehicle (LMV) Rs. 1000, For Medium passenger goods vehicle Rs. 2000" },
    { id: "9", title: "Violation of Road Regulations", amount: "₹ 1000" },
    { id: "10", title: "Driving When Mentally or Physically Unfit to Drive", amount: "₹ 1000",details:"For first offence Rs. 1000, For repeat offence Rs. 2000" },
     { id: "11", title: "Driving Without Insurance", amount: "₹ 2000" },
    { id: "12", title: "Overloading", amount: "₹ 2000" },
    { id: "13", title: "Driving Uninsured Vehicle", amount: "₹ 2000" },
     { id: "14", title: "Driving Vehicle Without Registration", amount: "₹ 2000" },
  
      { id: "15", title: "Dangerous/Rash Driving", amount: "₹ 5000",details:"For first offence Rs. 5000, For repeat offence Rs. 10000" },
    { id: "16", title: "Disobey of Traffic Signals", amount: "₹ 5000",details:"For first offence Rs. 5000, For repeat offence Rs. 10000" },
    { id: "17", title: "Using a Mobile Phone While Driving", amount: "₹ 5000",details:"For the first offence Rs. 5000, For repeat offence Rs. 10000" },
    { id: "18", title: "Racing", amount: "₹ 5000",details:"For the first offence Rs. 5000, For repeat offence Rs. 10000" },
    { id: "19", title: "Driving Without a Valid Driving Licence", amount: "₹ 5000" },
    { id: "20", title: "Drunk Driving", amount: "₹ 10,000" },
    { id: "21", title: "Carrying Explosive/Inflammable Substances", amount: "₹ 10,000" },
    { id: "22", title: "Not Giving Passage to Emergency Vehicles", amount: "₹ 10,000" },
    { id: "23", title: "Disqualified Person Driving \na Vehicle", amount: "₹ 10,000" },
     { id: "24", title: "Minor Driving Vehicle", amount: "₹ 25,000" },
    
    
  ];
  const toggleExpand = (id) => {
    if (expandedIds.includes(id)) {
      setExpandedIds(expandedIds.filter((item) => item !== id));
    } else {
      setExpandedIds([...expandedIds, id]);
    }
  };

  return (
    <View style={{ flex: 1, backgroundColor: "#FFFFFF" }}>
      {/* Header */}
      <View style={{ backgroundColor: "#1A9E75", height: 88, flexDirection: "row", alignItems: "center", paddingHorizontal: 13, paddingTop: 25 }}>
        <TouchableOpacity onPress={() => navigation.goBack()}>
          <Ionicons name="arrow-back" size={24} color="white" />
        </TouchableOpacity>
        <Text style={{ fontSize: 16, color: "#FFFBFB", marginLeft: 12,fontFamily:"Poppins-SemiBold" }}>
          Traffic Fines
        </Text>
      </View>
      
      <ScrollView contentContainerStyle={{ flexGrow: 1, paddingBottom: 20 }} nestedScrollEnabled={true}>
  
    <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 9,
          
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#EEEEEE",
          borderWidth:1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          width: "90%",
          marginLeft:20,
          marginTop:18,
        }}
      >
        
        <Text style={{ color:"#AFAFAF", fontSize: 14,marginLeft:10,fontFamily:"Poppins-Regular" }}>
          {selectedState || "Select Your State"}
        </Text>
        <Ionicons name="caret-down-sharp" size={20} color="#27A47D" />
      </TouchableOpacity>
      

      {/* Modal for State Selection */}
      <Modal
        visible={modalVisible}
        animationType="slide"
        transparent={true}
        onRequestClose={() => setModalVisible(false)}
      >
        <View style={{ flex: 1, justifyContent: "flex-end", backgroundColor: "rgba(0,0,0,0.3)" }}>
          <View style={{
            backgroundColor: "white",
            borderTopLeftRadius: 20,
            borderTopRightRadius: 20,
            padding: 20,
            maxHeight: "80%"
          }}>
            
            {/* Header with Close Button */}
            <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
              <Text style={{ fontSize: 18, fontWeight: "bold", color: "#333",marginBottom:14 }}>Choose State</Text>
              <TouchableOpacity onPress={() => setModalVisible(false)}>
                <Ionicons name="close-circle" size={24} color="green" />
              </TouchableOpacity>
            </View>
            

            {/* State List */}
            <FlatList
                data={states}
                keyExtractor={(item) => item}
                renderItem={({ item }) => (
                  <TouchableOpacity
                    style={{ flexDirection: "row", alignItems: "center", paddingVertical: 4 }}
                    onPress={() => {
                      setModalVisible(false);
                      navigation.navigate("StateChallanScreen", { stateName: item });
                    }}
                  >
                    <Ionicons name="radio-button-off" size={20} color="#1A9E75" style={{ marginRight: 10 }} />
                    <Text style={{ fontSize: 16, color: "#333" }}>{item}</Text>
                  </TouchableOpacity>
                )}
                showsVerticalScrollIndicator={false}
              />
              

          </View>
        </View>
      </Modal>
      

    {/* Traffic light */}
    <View style={{
          flexDirection: "row",
          alignItems: "center",
          backgroundColor: "#F0FFFA", 
          padding: 6, 
          borderRadius: 12,
          width: "91%", 
          alignSelf: "center", 
          marginTop: 10, 
        }}>
  <MaterialCommunityIcons name="traffic-light" size={44} color="#1A9E75" paddingHorizontal="5" />
  
  {/* Text Container */}
  <View style={{ flex: 1, marginLeft: 1 }}>  
    <Text style={{
      fontSize: 16, 
      fontWeight: "Medium",
      color: "#333",
      fontFamily: "Poppins",
      
    }}>
      Traffic fines in Maharashtra 
    </Text>
    
    <Text style={{
      fontSize: 8, 
      color: "#393939",
      marginTop: 4,
      fontFamily: "Poppins-Regular",
    }}>
      List of traffic fines as per Motor Vehicle (Amendment) Act
    </Text>
  </View>
</View>


<View style={{ flex: 1, backgroundColor: "#FFFFFF", padding: 10,width:"100%",marginTop:9 }}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        contentContainerStyle={{ paddingBottom: 15 }}
        nestedScrollEnabled={true}
        scrollEnabled={false}
        renderItem={({ item }) => {
          const isExpanded = expandedIds.includes(item.id);
          return (
            
            <View
              style={{
                backgroundColor: "#FFFFFF",
                borderRadius: 15,
                marginBottom: 13,
                elevation: 3,
                width: "96%",
                alignSelf: "center",
                paddingHorizontal: 15,
                paddingVertical: 10,
                borderColor: "#E4E4E4",
                borderWidth: 1,
                minHeight: 60, 
                justifyContent: "center",
              }}
            >
              <TouchableOpacity onPress={() => toggleExpand(item.id)} style={{
                  flexDirection: "row",alignItems: "center",justifyContent: "space-between",position: "relative", 
                }}
              >
                <View  style={{width:"60%"}}>
                    <Text style={{fontSize: 14,color: "#393939",flex: 1,fontFamily:"Poppins-Regular",textAlign: "left",lineHeight: 20,
                    
                    }}
                    >
                      {item.title}
                    </Text>
                </View>

                 
                 <View
                      style={{
                        height: 27,
                        width: 1,
                        backgroundColor: "#E4E4E4",
                        position: "absolute",
                        left: 175, 
                        top: "50%",
                        transform: [{ translateY: -13.5 }], 
                      }}
                    />

                    <View style={{ flex: 1, alignItems: "center",justifyContent: "center",width:"40%",paddingHorizontal:20 }}>
                        <Text
                          style={{
                            fontSize: 14,
                            fontWeight: "Medium",
                            color: "#1A9E75",
                            textAlign: "center",
                            marginLeft:5,

                          }}
                        >
                          {item.amount}
                        </Text>
                      </View>
                <View style={{ flex: 0.3,alignItems: "flex-end"}}>
                {item.details && (
                <Ionicons
                  name={isExpanded ? "chevron-up" : "chevron-down"}
                  size={20} color="#393939" marginLeft={-20}/>
                )}
                </View>
              </TouchableOpacity>

              {isExpanded && item.details && (
                <View
                  style={{
                    backgroundColor: "white",
                    padding: 8,
                    borderRadius: 8,
                    marginTop: 5,
                  }}
                >
                  <Text style={{ fontSize: 14, color: "#A0A0A0",fontFamily: "Poppins-Regular" }}>
                    {item.details}
                  </Text>
                </View>
              )}
            </View>
          )
        }
      }
      />
      </View>
      </ScrollView>
    </View>
  )
}


  const ChallanDetailsScreen = ({ navigation }) => {
    const [activeTab, setActiveTab] = useState("Pending");
  
    return (
      <View style={{ flex: 1, backgroundColor: "#fff" }}>
        
        <View style={{ backgroundColor: "#1A9E75", height: 80, flexDirection: "row", alignItems: "center", paddingHorizontal: 16, paddingTop: 30 }}>
          <TouchableOpacity onPress={() => navigation.goBack()}>
            <Ionicons name="arrow-back" size={24} color="white" />
          </TouchableOpacity>
          <Text style={{ fontSize: 16, fontWeight: "700", color: "white", marginLeft: 13,fontFamily: 'Poppins', }}>Challan Details</Text>
        </View>
  
        
        <View style={{ flexDirection: "row", justifyContent: "space-around", paddingHorizontal:15,marginTop: 2,borderBottomWidth: 1, borderColor: "#ddd",marginLeft:10,marginRight:10, }}>
        {/* overflow: "hidden", */}
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 3 : 0, borderBottomColor: "#1A9E75", paddingHorizontal: 10,
    borderBottomStartRadius: 10,
    borderBottomEndRadius: 10,
    borderTopStartRadius:10,
    borderTopEndRadius:10

     }}>
            <Text style={{ fontSize: 16,marginLeft:1,fontFamily: 'Poppins-Medium', color: activeTab === "Pending" ? "#1A9E75" : "#393939" }}>Pending</Text>
          </TouchableOpacity>
          <View
          style={{
            height: 27,
            width: 1,
            backgroundColor: "#E4E4E4",
            marginLeft: "-1%",
            alignSelf: "center",
          }}
        />
        
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 3 : 0, borderBottomColor: "#1A9E75",paddingHorizontal: 20,
            borderBottomStartRadius: 10,
            borderBottomEndRadius: 10,
            borderTopStartRadius:10,
            borderTopEndRadius:10
          }}>
            <Text style={{ fontSize: 16,fontFamily: 'Poppins-Medium', color: activeTab === "Paid" ? "#1A9E75" : "#393939" }}>Paid</Text>
          </TouchableOpacity>
        
        </View>
  
      
        {activeTab === "Pending" ? (
          <FlatList
            data={challans}
            keyExtractor={(item) => item.id}
            renderItem={({ item }) => (
              <View style={{backgroundColor: "white", margin: 10, padding: 15, borderRadius: 15, elevation: 2,marginTop:22,marginBottom:-6}}>
      
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <Text style={{fontSize: 16,  color: "#393939",fontFamily:"Poppins-Regular"}}>{item.vehicleNumber}</Text>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#1A9E75",fontFamily:"Poppins-Medium"}}>
                {item.amount} 
                </Text>
              </View>

              {/* Due Date */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                
                <Text style={{ fontSize: 12, color: "#A0A0A0", flex: 1 }}>
                  Due Date: {item.dueDate}
                </Text>
        
                <LinearGradient 
                colors={["#E44E2D", "#E4852E"]} 
                start={{ x: 0, y: 0 }} 
                end={{ x: 0, y: 1 }} 
                style={{paddingHorizontal: 1,
                  paddingVertical: 0.5,
                  borderRadius: 3,
                  alignSelf: "flex-start",
                fontFamily:"Poppins"}}
              >
              <Text style={{ fontSize: 10, color: "white",fontWeight:"Medium" }}>{item.status}</Text>
                </LinearGradient>
              </View>

              {/* Divider */}
              <View style={{height: 1, backgroundColor: "#ddd", marginVertical: 8,marginTop:15,marginBottom:10 }} />

              
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <View style={{backgroundColor: "#F5F5F5", paddingVertical: 5, paddingHorizontal: 7, borderRadius: 10}}>
                  <Text style={{fontSize: 10, color: "#393939",fontFamily:"Poppins-Regular"}}>{item.challanId}</Text>
                </View>

                <View style={{flexDirection: "row", alignItems: "center"}}>
                  <TouchableOpacity style={{flexDirection: "row", alignItems: "center", marginRight: 10}}>
                    <Text style={{color: "#1A9E75", fontSize: 12,fontFamily:"Poppins-Medium"}}>More Info</Text>
                    <Ionicons name="chevron-down" size={14} color="#1A9E75" />
                  </TouchableOpacity>

                  <TouchableOpacity style={{backgroundColor: "#1A9E75", paddingVertical: 5, paddingHorizontal: 11, borderRadius: 8}}>
                    <Text style={{color: "#F0FFFA",  fontSize: 10,fontFamily:"Poppins-Medium"}}>Pay Now</Text>
                  </TouchableOpacity>
                </View>
              </View>
              </View>
            )}
          />
        ) : (

          <View style={{ flex: 1,
            justifyContent: "center",
            alignItems: "center",}}>
          <Image source={require("../../assets/images/challan.jpg")} style={{width: 150,
                                                                              height: 150,
                                                                              resizeMode: "contain",}} />
        <Text style={{marginTop: 10,
                        fontSize: 14,
                        color: "#444",}}>Checking for challan...</Text>
        </View>
        )}
      </View>
      
    );
  };

export default function App() {
  return (
    <NavigationIndependentTree>
    <NavigationContainer>
      <Stack.Navigator initialRouteName='PayChallanScreen'>
        <Stack.Screen name="PayChallanScreen" component={PayChallanScreen} options={{ headerShown: false }} />
        <Stack.Screen name="StateChallanScreen" component={StateChallanScreen} options={{ headerShown: false }} />
        <Stack.Screen name="ChallanDetailsScreen" component={ChallanDetailsScreen} options={{ headerShown: false }} />
      </Stack.Navigator>
      <StatusBar barStyle="light-content" backgroundColor="#1A9E75" />
    </NavigationContainer>
    </NavigationIndependentTree>
  );
}



const styles = StyleSheet.create({
  cardContainer: {
    padding: 20,
    borderRadius: 15,
    width: "90%", 
    marginTop: 15,
    alignSelf: "center",
    marginBottom: 5,
    elevation: 5, 
    shadowColor: "#000", 
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.2,
    shadowRadius: 4,
    backgroundColor: "#0BC189",
  },
  fullWidthTextContainer: {
    width: "100%", 
    alignItems: "center",
    marginBottom: 5,
  },
  inviteText: {
    color: "#FBFF29",
    fontSize: 15,
    fontFamily: "Arial",
    textAlign: "center",
  },
  rowContainer: {
    flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",
  },
  textContainer: {
    flex: 1,
    paddingRight: 10,
  },
  titleText: {
    color: "white",
    fontSize: 20,
    fontFamily: "Arial",
    fontWeight: "bold",
    marginBottom: 5,
  },
  descriptionText: {
    color: "white",
    fontSize: 12,
    marginBottom: 10,
  },
  inviteButton: {
    backgroundColor: "white",
    paddingVertical: 8,
    paddingHorizontal: 15,
    borderRadius: 12,
    alignItems: "center",
    alignSelf: "flex-start",
  },
  inviteButtonText: {
    color: "#0BC189",
    fontWeight: "bold",
  },
  imageContainer: {
    width: 80,
    height: 100,
    alignItems: "center",
    justifyContent: "center",
    paddingTop: 40, 
  },
  image: {
    width: 70,
    height: 90,
    resizeMode: "contain",
  },

});
