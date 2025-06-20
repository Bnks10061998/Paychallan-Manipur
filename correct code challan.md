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
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 3 : 0, borderBottomColor: "#1A9E75", paddingHorizontal: 10,borderTopLeftRadius: 8,  
      borderTopRightRadius: 6, }}>
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
        
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 3 : 0, borderBottomColor: "#1A9E75",paddingHorizontal: 20,borderTopLeftRadius: 15,borderTopRightRadius: 15,}}>
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
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 3 : 0, borderBottomColor: "#1A9E75", paddingHorizontal: 10,borderTopLeftRadius: 8,  
      borderTopRightRadius: 6, }}>
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
        
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 3 : 0, borderBottomColor: "#1A9E75",paddingHorizontal: 20,borderTopLeftRadius: 15,borderTopRightRadius: 15,}}>
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
                end={{ x: 1, y: 0 }} 
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

        \
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
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 3 : 0, borderBottomColor: "#1A9E75", paddingHorizontal: 10,borderTopLeftRadius: 8,  
      borderTopRightRadius: 6, }}>
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
        
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 3 : 0, borderBottomColor: "#1A9E75",paddingHorizontal: 20,borderTopLeftRadius: 15,borderTopRightRadius: 15,}}>
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
                end={{ x: 1, y: 0 }} 
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
        // fontWeight: "bold",
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
   {/* Error Message (Only show if there's an error) */}
   {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5,alignSelf: "flex-start" }}>
            {errorMessage}
          </Text>
        )}
</View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8,marginTop: -5,fontFamily: "Poppins-Medium", }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        {/* Step 1 */}
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
    {/* Text (Number Plate Content) */}
    <SvgText x="90" y="57" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "Medium", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14,fontFamily: "Poppins_Regular", }}>Enter your {"\n"} vehicle number</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        {/* Step 2 */}
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
    {/* Document Shape with Right-Bottom Cut */}
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />
    {/* Folded Corner */}
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    {/* Text Lines */}
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    {/* Challan Label */}
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14,fontWeight: "Medium",fontFamily: "Poppins-Medium", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14,fontFamily: "Poppins-Regular", }}>Check Your {"\n"} Challans</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        {/* Step 3 */}
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
        
      {/* Button to Open Modal */}
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

      {/* Main Content - Text and Image */}
      <View style={{flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",}}>
        {/* Left Section - Text Content */}
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

          {/* Invite Now Button */}
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

        {/* Right Section - Image */}
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
    {/* Scroll Select your State in Fines */}
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
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 3 : 0, borderBottomColor: "#1A9E75", paddingHorizontal: 10,borderTopLeftRadius: 8,  
      borderTopRightRadius: 6, }}>
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
        
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 3 : 0, borderBottomColor: "#1A9E75",paddingHorizontal: 20,borderTopLeftRadius: 15,borderTopRightRadius: 15,}}>
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
                end={{ x: 1, y: 0 }} 
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

              {/* Challan ID & Buttons */}
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
























import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView, Modal, FlatList,Image,StyleSheet,Share,StatusBar   } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { LinearGradient } from 'expo-linear-gradient';
import * as Font from "expo-font";
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
  const [expanded, setExpanded] = useState(null);
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
  <TouchableOpacity onPress={() => navigation.goBack()} style={{ position: "absolute", left: 17 }}>
    <Ionicons name="arrow-back" size={24} color="white" />
  </TouchableOpacity>
  <Text 
    style={{ 
      fontSize: 16, 
      fontWeight: "700", 
      color: "#FFFBFB", 
      fontFamily: 'Poppins',
      marginLeft: 56, 
    }}
  >
    Pay Challan
  </Text>
</View>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 15, marginTop: 17, elevation: 2, marginHorizontal: 20,paddingBottom: 20,paddingTop: 11 }}>
        <Text style={{ fontSize: 16, fontWeight: "medium", marginBottom: 5,fontFamily: 'Poppins' }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8,fontWeight: "Normal",lineHeight:20  }}>Enter your vehicle number and check {"\n"}your challans</Text>
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
        backgroundColor: "#E6F9F0",
        borderWidth: 1.5,
        borderColor: isValid ? "#1A9E75" : "red",
        borderColor: "#1A9E75",
        borderRadius: 8,
        paddingHorizontal: 8,
        color: "#1A9E75",
        fontWeight: "bold",
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
      <Text style={{ color: "white", fontWeight: "bold" }}>Check</Text>
    </TouchableOpacity>
  </View>
   {/* Error Message (Only show if there's an error) */}
   {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5,alignSelf: "flex-start" }}>
            {errorMessage}
          </Text>
        )}
</View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Normal", marginBottom: 8,marginTop: -5 }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        {/* Step 1 */}
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
    {/* Text (Number Plate Content) */}
    <SvgText x="90" y="55" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Enter your {"\n"} vehicle number</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        {/* Step 2 */}
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
    {/* Document Shape with Right-Bottom Cut */}
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />
    {/* Folded Corner */}
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    {/* Text Lines */}
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    {/* Challan Label */}
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Check Your {"\n"} Challans</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        {/* Step 3 */}
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
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 3</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14 }}>Make {"\n"} Payment</Text>
        </View>
      </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, marginTop: 3, elevation: 2,width: "90%",marginHorizontal: 20 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8,fontFamily: 'Poppins-SemiBold', }}>Traffic Fines</Text>{/* ,color:"#393939"*/}
        <Text style={{fontSize: 14,
                      color: "#AFAFAF",
                      marginBottom: 8,marginTop: -3,}}>Select your state to see traffic fines</Text>
        
      {/* Button to Open Modal */}
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
      fontFamily: "Arial",
      
    }}>
      Invite your friends to make hassle-free
    </Text>
  </View>

      {/* Main Content - Text and Image */}
      <View style={{flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",}}>
        {/* Left Section - Text Content */}
        <View style={{flex: 1,
                      paddingRight: 10,}}>
          <Text style={{color: "white",
    fontSize: 18,
    fontFamily:"Poppins",
    fontWeight: "bold",
    marginBottom: 5,marginLeft:15}}>Challan Payments</Text>
          <Text style={{color: "white",
    fontSize: 10,
    marginBottom: 10,
    marginLeft:15}}>Pay using UPI, Debit & Credit Cards</Text>

          {/* Invite Now Button */}
          <TouchableOpacity style={{backgroundColor: "white",
                                    paddingVertical: 6,
                                    paddingHorizontal: 10,
                                    borderRadius: 15,
                                    alignItems: "center",
                                    alignSelf: "flex-start",marginLeft:15}} 
                                    onPress={shareReferral}>
          <Text style={{alignItems: "flex-end",fontSize:10,fontWeight:"bold",fontFamily:"Poppins"}}>Invite Now</Text>
        </TouchableOpacity>
        </View>

        {/* Right Section - Image */}
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
        <Text style={{fontSize: 18, fontWeight: "bold", color: "#333", marginBottom: 10}}>FAQs</Text>
        {faqs.map((faq, index) => (
          <View key={index} style={{backgroundColor: "#FFFFFF", borderRadius: 10, marginBottom: 10, padding: 10, elevation: 2}}>
            <TouchableOpacity style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center", paddingVertical: 10 }} onPress={() => toggleFAQ(index)}>
              <Text style={{fontSize: 15, fontWeight: "600", color: "#333", fontFamily: "Calibri",lineHeight:20}}>{faq.question}</Text>
              <Ionicons name={faq.collapsed ? "chevron-down" : "chevron-up"} size={20} color="#333" />
            </TouchableOpacity>
            <Collapsible collapsed={faq.collapsed}>
              <Text style={{fontSize: 16, color: "#A0A0A0", paddingTop: 4, paddingHorizontal: 5,lineHeight: 18,fontFamily: "Poppins-Regular"}}>{faq.answer}</Text>
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
    {/* Scroll Select your State in Fines */}
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
          <Text style={{ fontSize: 18, fontWeight: "700", color: "white", marginLeft: 16 }}>Challan Details</Text>
        </View>
  
        
        <View style={{ flexDirection: "row", justifyContent: "space-around", paddingHorizontal:18,marginTop: 10,borderBottomWidth: 1, borderColor: "#ddd",marginLeft:10,marginRight:10 }}>
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Pending" ? "#1A9E75" : "#666" }}>Pending</Text>
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
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Paid" ? "#1A9E75" : "#666" }}>Paid</Text>
          </TouchableOpacity>
          
        </View>
  
      
        {activeTab === "Pending" ? (
          <FlatList
            data={challans}
            keyExtractor={(item) => item.id}
            renderItem={({ item }) => (
              <View style={{backgroundColor: "white", margin: 10, padding: 15, borderRadius: 15, elevation: 2}}>
      
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#000"}}>{item.vehicleNumber}</Text>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#1A9E75"}}>
                {item.amount} 
                </Text>
              </View>

              {/* Due Date */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                
                <Text style={{ fontSize: 14, color: "#888", flex: 1 }}>
                  Due Date: {item.dueDate}
                </Text>
        
                <LinearGradient 
                colors={["#E44E2D", "#E4852E"]} 
                start={{ x: 0, y: 0 }} 
                end={{ x: 1, y: 0 }} 
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
              <View style={{height: 1, backgroundColor: "#ddd", marginVertical: 8 }} />

              {/* Challan ID & Buttons */}
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <View style={{backgroundColor: "#F5F5F5", paddingVertical: 8, paddingHorizontal: 10, borderRadius: 10}}>
                  <Text style={{fontSize: 11, color: "#393939"}}>{item.challanId}</Text>
                </View>

                <View style={{flexDirection: "row", alignItems: "center"}}>
                  <TouchableOpacity style={{flexDirection: "row", alignItems: "center", marginRight: 10}}>
                    <Text style={{color: "#1A9E75", fontSize: 12}}>More Info</Text>
                    <Ionicons name="chevron-down" size={14} color="#1A9E75" />
                  </TouchableOpacity>

                  <TouchableOpacity style={{backgroundColor: "#1A9E75", paddingVertical: 5, paddingHorizontal: 11, borderRadius: 8}}>
                    <Text style={{color: "#F0FFFA", fontFamily:"Medium", fontSize: 10}}>Pay Now</Text>
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

































import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView, Modal, FlatList,Image,StyleSheet,Share,StatusBar   } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { LinearGradient } from 'expo-linear-gradient';
import * as Font from "expo-font";
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
  const [expanded, setExpanded] = useState(null);
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
  <TouchableOpacity onPress={() => navigation.goBack()} style={{ position: "absolute", left: 17 }}>
    <Ionicons name="arrow-back" size={24} color="white" />
  </TouchableOpacity>
  <Text 
    style={{ 
      fontSize: 16, 
      fontWeight: "700", 
      color: "#FFFBFB", 
      fontFamily: 'Poppins',
      marginLeft: 56, 
    }}
  >
    Pay Challan
  </Text>
</View>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 15, marginTop: 17, elevation: 2, marginHorizontal: 20,paddingBottom: 20,paddingTop: 11 }}>
        <Text style={{ fontSize: 16, fontWeight: "medium", marginBottom: 5,fontFamily: 'Poppins' }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8,fontWeight: "Normal",lineHeight:20  }}>Enter your vehicle number and check {"\n"}your challans</Text>
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
        backgroundColor: "#E6F9F0",
        borderWidth: 1.5,
        borderColor: isValid ? "#1A9E75" : "red",
        borderColor: "#1A9E75",
        borderRadius: 8,
        paddingHorizontal: 8,
        color: "#1A9E75",
        fontWeight: "bold",
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
      <Text style={{ color: "white", fontWeight: "bold" }}>Check</Text>
    </TouchableOpacity>
  </View>
   {/* Error Message (Only show if there's an error) */}
   {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5,alignSelf: "flex-start" }}>
            {errorMessage}
          </Text>
        )}
</View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Normal", marginBottom: 8,marginTop: -5 }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        {/* Step 1 */}
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
    {/* Text (Number Plate Content) */}
    <SvgText x="90" y="55" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Enter your {"\n"} vehicle number</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        {/* Step 2 */}
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
    {/* Document Shape with Right-Bottom Cut */}
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />
    {/* Folded Corner */}
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    {/* Text Lines */}
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    {/* Challan Label */}
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Check Your {"\n"} Challans</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        {/* Step 3 */}
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
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 3</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14 }}>Make {"\n"} Payment</Text>
        </View>
      </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, marginTop: 3, elevation: 2,width: "90%",marginHorizontal: 20 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8 }}>Traffic Fines</Text>{/* ,color:"#393939"*/}
        <Text style={{fontSize: 14,
                      color: "#AFAFAF",
                      marginBottom: 8,marginTop: -3}}>Select your state to see traffic fines</Text>
        
      {/* Button to Open Modal */}
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
      fontFamily: "Arial",
      
    }}>
      Invite your friends to make hassle-free
    </Text>
  </View>

      {/* Main Content - Text and Image */}
      <View style={{flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",}}>
        {/* Left Section - Text Content */}
        <View style={{flex: 1,
                      paddingRight: 10,}}>
          <Text style={{color: "white",
    fontSize: 18,
    fontFamily:"Poppins",
    fontWeight: "bold",
    marginBottom: 5,marginLeft:15}}>Challan Payments</Text>
          <Text style={{color: "white",
    fontSize: 10,
    marginBottom: 10,
    marginLeft:15}}>Pay using UPI, Debit & Credit Cards</Text>

          {/* Invite Now Button */}
          <TouchableOpacity style={{backgroundColor: "white",
                                    paddingVertical: 6,
                                    paddingHorizontal: 10,
                                    borderRadius: 15,
                                    alignItems: "center",
                                    alignSelf: "flex-start",marginLeft:15}} 
                                    onPress={shareReferral}>
          <Text style={{alignItems: "flex-end",fontSize:10,fontWeight:"bold",fontFamily:"Poppins"}}>Invite Now</Text>
        </TouchableOpacity>
        </View>

        {/* Right Section - Image */}
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
        <Text style={{fontSize: 18, fontWeight: "bold", color: "#333", marginBottom: 10}}>FAQs</Text>
        {faqs.map((faq, index) => (
          <View key={index} style={{backgroundColor: "#FFFFFF", borderRadius: 10, marginBottom: 10, padding: 10, elevation: 2}}>
            <TouchableOpacity style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center", paddingVertical: 10 }} onPress={() => toggleFAQ(index)}>
              <Text style={{fontSize: 15, fontWeight: "600", color: "#333", fontFamily: "Calibri",lineHeight:20}}>{faq.question}</Text>
              <Ionicons name={faq.collapsed ? "chevron-down" : "chevron-up"} size={20} color="#333" />
            </TouchableOpacity>
            <Collapsible collapsed={faq.collapsed}>
              <Text style={{fontSize: 16, color: "#A0A0A0", paddingTop: 4, paddingHorizontal: 5,lineHeight: 18,fontFamily: "Poppins-Regular"}}>{faq.answer}</Text>
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
    { id: "2", title: "Driving Without a Number Plate", amount: "₹ 500",details:"	For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "3", title: "Parking in No-parking Zone", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs.1500" },
    { id: "4", title: "Failure to Intimate Change of the Address of Vehicle Owner", amount: "₹ 500",details:"For the first offence Rs. 500, For repeat offence Rs. 1500" },
  
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
    { id: "23", title: "Disqualified Person Driving a Vehicle", amount: "₹ 10,000" },
     { id: "24", title: "Minor Driving Vehicle", amount: "₹ 25,000" },
    // { id: "25", title: "Driving a Vehicle \nRegistered in Another \nState for More Than 12 Months", amount: "₹ 500" },
    
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
        <Text style={{ fontSize: 16, fontWeight: "700", color: "#FFFBFB", marginLeft: 12,fontFamily:"Poppins" }}>
          Traffic Fines
        </Text>
      </View>
      
      <ScrollView contentContainerStyle={{ flexGrow: 1, paddingBottom: 20 }} nestedScrollEnabled={true}>
    {/* Scroll Select your State in Fines */}
    <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 10,
          
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#EEEEEE",
          borderWidth:1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          width: "90%",
          marginLeft:20,
          marginTop:20,
        }}
      >
        
        <Text style={{ color:"#AFAFAF", fontSize: 14,marginLeft:10,fontFamily:"Poppins" }}>
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
          padding: 9, 
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
      fontWeight: "bold",

      color: "#333",
      fontFamily: "Poppins_600SemiBold",
      
    }}>
      Traffic fines in Maharashtra 
    </Text>
    
    <Text style={{
      fontSize: 8, 
      color: "#666",
      marginTop: 5,
      fontFamily: "Poppins_400Regular",
    }}>
      List of traffic fines as per Motor Vehicle (Amendment) Act
    </Text>
  </View>
</View>


<View style={{ flex: 1, backgroundColor: "#FFFFFF", padding: 10,width:"100%" }}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        contentContainerStyle={{ paddingBottom: 20 }}
        nestedScrollEnabled={true}
        scrollEnabled={false}
        renderItem={({ item }) => {
          const isExpanded = expandedIds.includes(item.id);
          return (
            
            <View
              style={{
                backgroundColor: "#FFFFFF",
                borderRadius: 15,
                marginBottom: 10,
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
                    <Text style={{fontSize: 14,color: "#393939",flex: 1,fontFamily:"Poppins",textAlign: "left",lineHeight: 20,
                    
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
                            fontWeight: "600",
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
                  <Text style={{ fontSize: 15, color: "#A0A0A0" }}>
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
          <Text style={{ fontSize: 18, fontWeight: "700", color: "white", marginLeft: 16 }}>Challan Details</Text>
        </View>
  
        
        <View style={{ flexDirection: "row", justifyContent: "space-around", paddingHorizontal:18,marginTop: 10,borderBottomWidth: 1, borderColor: "#ddd",marginLeft:10,marginRight:10 }}>
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Pending" ? "#1A9E75" : "#666" }}>Pending</Text>
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
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Paid" ? "#1A9E75" : "#666" }}>Paid</Text>
          </TouchableOpacity>
          
        </View>
  
      
        {activeTab === "Pending" ? (
          <FlatList
            data={challans}
            keyExtractor={(item) => item.id}
            renderItem={({ item }) => (
              <View style={{backgroundColor: "white", margin: 10, padding: 15, borderRadius: 15, elevation: 2}}>
      
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#000"}}>{item.vehicleNumber}</Text>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#1A9E75"}}>
                {item.amount} 
                </Text>
              </View>

              {/* Due Date */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                
                <Text style={{ fontSize: 14, color: "#888", flex: 1 }}>
                  Due Date: {item.dueDate}
                </Text>
        
                <LinearGradient 
                colors={["#E44E2D", "#E4852E"]} 
                start={{ x: 0, y: 0 }} 
                end={{ x: 1, y: 0 }} 
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
              <View style={{height: 1, backgroundColor: "#ddd", marginVertical: 8 }} />

              {/* Challan ID & Buttons */}
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <View style={{backgroundColor: "#F5F5F5", paddingVertical: 8, paddingHorizontal: 10, borderRadius: 10}}>
                  <Text style={{fontSize: 11, color: "#393939"}}>{item.challanId}</Text>
                </View>

                <View style={{flexDirection: "row", alignItems: "center"}}>
                  <TouchableOpacity style={{flexDirection: "row", alignItems: "center", marginRight: 10}}>
                    <Text style={{color: "#1A9E75", fontSize: 12}}>More Info</Text>
                    <Ionicons name="chevron-down" size={14} color="#1A9E75" />
                  </TouchableOpacity>

                  <TouchableOpacity style={{backgroundColor: "#1A9E75", paddingVertical: 5, paddingHorizontal: 11, borderRadius: 8}}>
                    <Text style={{color: "#F0FFFA", fontFamily:"Medium", fontSize: 10}}>Pay Now</Text>
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






































import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView, Modal, FlatList,Image,StyleSheet,Share,StatusBar   } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { LinearGradient } from 'expo-linear-gradient';
import * as Font from "expo-font";
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
  const [expanded, setExpanded] = useState(null);
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
  <TouchableOpacity onPress={() => navigation.goBack()} style={{ position: "absolute", left: 17 }}>
    <Ionicons name="arrow-back" size={24} color="white" />
  </TouchableOpacity>
  <Text 
    style={{ 
      fontSize: 16, 
      fontWeight: "700", 
      color: "#FFFBFB", 
      fontFamily: 'Poppins',
      marginLeft: 56, 
    }}
  >
    Pay Challan
  </Text>
</View>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 15, marginTop: 17, elevation: 2, marginHorizontal: 20,paddingBottom: 20,paddingTop: 11 }}>
        <Text style={{ fontSize: 16, fontWeight: "medium", marginBottom: 5,fontFamily: 'Poppins' }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8,fontWeight: "Normal",lineHeight:20  }}>Enter your vehicle number and check {"\n"}your challans</Text>
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
        backgroundColor: "#E6F9F0",
        borderWidth: 1.5,
        borderColor: isValid ? "#1A9E75" : "red",
        borderColor: "#1A9E75",
        borderRadius: 8,
        paddingHorizontal: 8,
        color: "#1A9E75",
        fontWeight: "bold",
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
      <Text style={{ color: "white", fontWeight: "bold" }}>Check</Text>
    </TouchableOpacity>
  </View>
   {/* Error Message (Only show if there's an error) */}
   {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5,alignSelf: "flex-start" }}>
            {errorMessage}
          </Text>
        )}
</View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Normal", marginBottom: 8,marginTop: -5 }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        {/* Step 1 */}
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
    {/* Text (Number Plate Content) */}
    <SvgText x="90" y="55" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Enter your {"\n"} vehicle number</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        {/* Step 2 */}
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
    {/* Document Shape with Right-Bottom Cut */}
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />
    {/* Folded Corner */}
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    {/* Text Lines */}
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    {/* Challan Label */}
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Check Your {"\n"} Challans</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        {/* Step 3 */}
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
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 3</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14 }}>Make {"\n"} Payment</Text>
        </View>
      </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, marginTop: 3, elevation: 2,width: "90%",marginHorizontal: 20 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8 }}>Traffic Fines</Text>{/* ,color:"#393939"*/}
        <Text style={{fontSize: 14,
                      color: "#AFAFAF",
                      marginBottom: 8,marginTop: -3}}>Select your state to see traffic fines</Text>
        
      {/* Button to Open Modal */}
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
      fontFamily: "Arial",
      
    }}>
      Invite your friends to make hassle-free
    </Text>
  </View>

      {/* Main Content - Text and Image */}
      <View style={{flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",}}>
        {/* Left Section - Text Content */}
        <View style={{flex: 1,
                      paddingRight: 10,}}>
          <Text style={{color: "white",
    fontSize: 18,
    fontFamily:"Poppins",
    fontWeight: "bold",
    marginBottom: 5,marginLeft:15}}>Challan Payments</Text>
          <Text style={{color: "white",
    fontSize: 10,
    marginBottom: 10,
    marginLeft:15}}>Pay using UPI, Debit & Credit Cards</Text>

          {/* Invite Now Button */}
          <TouchableOpacity style={{backgroundColor: "white",
                                    paddingVertical: 6,
                                    paddingHorizontal: 10,
                                    borderRadius: 15,
                                    alignItems: "center",
                                    alignSelf: "flex-start",marginLeft:15}} 
                                    onPress={shareReferral}>
          <Text style={{alignItems: "flex-end",fontSize:10,fontWeight:"bold",fontFamily:"Poppins"}}>Invite Now</Text>
        </TouchableOpacity>
        </View>

        {/* Right Section - Image */}
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
        <Text style={{fontSize: 18, fontWeight: "bold", color: "#333", marginBottom: 10}}>FAQs</Text>
        {faqs.map((faq, index) => (
          <View key={index} style={{backgroundColor: "#FFFFFF", borderRadius: 10, marginBottom: 10, padding: 10, elevation: 2}}>
            <TouchableOpacity style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center", paddingVertical: 10 }} onPress={() => toggleFAQ(index)}>
              <Text style={{fontSize: 15, fontWeight: "600", color: "#333", fontFamily: "Calibri",lineHeight:20}}>{faq.question}</Text>
              <Ionicons name={faq.collapsed ? "chevron-down" : "chevron-up"} size={20} color="#333" />
            </TouchableOpacity>
            <Collapsible collapsed={faq.collapsed}>
              <Text style={{fontSize: 16, color: "#A0A0A0", paddingTop: 4, paddingHorizontal: 5,lineHeight: 18,fontFamily: "Poppins-Regular"}}>{faq.answer}</Text>
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
    { id: "2", title: "Driving Without a Number Plate", amount: "₹ 500",details:"	For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "3", title: "Parking in No-parking Zone", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs.1500" },
    { id: "4", title: "Failure to Intimate Change of the Address of Vehicle Owner", amount: "₹ 500",details:"For the first offence Rs. 500, For repeat offence Rs. 1500" },
  
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
    { id: "23", title: "Disqualified Person Driving a Vehicle", amount: "₹ 10,000" },
     { id: "24", title: "Minor Driving Vehicle", amount: "₹ 25,000" },
    // { id: "25", title: "Driving a Vehicle \nRegistered in Another \nState for More Than 12 Months", amount: "₹ 500" },
    
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
        <Text style={{ fontSize: 16, fontWeight: "700", color: "#FFFBFB", marginLeft: 12,fontFamily:"Poppins" }}>
          Traffic Fines
        </Text>
      </View>
      
      <ScrollView contentContainerStyle={{ flexGrow: 1, paddingBottom: 20 }} nestedScrollEnabled={true}>
    {/* Scroll Select your State in Fines */}
    <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 10,
          
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#EEEEEE",
          borderWidth:1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          width: "90%",
          marginLeft:20,
          marginTop:20,
        }}
      >
        
        <Text style={{ color:"#AFAFAF", fontSize: 14,marginLeft:10,fontFamily:"Poppins" }}>
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
          padding: 9, 
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
      fontWeight: "bold",

      color: "#333",
      fontFamily: "Poppins_600SemiBold",
      
    }}>
      Traffic fines in Maharashtra 
    </Text>
    
    <Text style={{
      fontSize: 8, 
      color: "#666",
      marginTop: 5,
      fontFamily: "Poppins_400Regular",
    }}>
      List of traffic fines as per Motor Vehicle (Amendment) Act
    </Text>
  </View>
</View>


<View style={{ flex: 1, backgroundColor: "#FFFFFF", padding: 10,width:"100%" }}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        contentContainerStyle={{ paddingBottom: 20 }}
        nestedScrollEnabled={true}
        scrollEnabled={false}
        renderItem={({ item }) => {
          const isExpanded = expandedIds.includes(item.id);
          return (
            
            <View
              style={{
                backgroundColor: "#FFFFFF",
                borderRadius: 15,
                marginBottom: 10,
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
                    <Text style={{fontSize: 14,color: "#393939",flex: 1,fontFamily:"Poppins",textAlign: "left",lineHeight: 20,
                    
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
                            fontWeight: "600",
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
                  <Text style={{ fontSize: 15, color: "#A0A0A0" }}>
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
          <Text style={{ fontSize: 18, fontWeight: "700", color: "white", marginLeft: 16 }}>Challan Details</Text>
        </View>
  
        
        <View style={{ flexDirection: "row", justifyContent: "space-around", paddingHorizontal:18,marginTop: 10,borderBottomWidth: 1, borderColor: "#ddd",marginLeft:10,marginRight:10 }}>
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Pending" ? "#1A9E75" : "#666" }}>Pending</Text>
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
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Paid" ? "#1A9E75" : "#666" }}>Paid</Text>
          </TouchableOpacity>
          
        </View>
  
      
        {activeTab === "Pending" ? (
          <FlatList
            data={challans}
            keyExtractor={(item) => item.id}
            renderItem={({ item }) => (
              <View style={{backgroundColor: "white", margin: 10, padding: 15, borderRadius: 15, elevation: 2}}>
      
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#000"}}>{item.vehicleNumber}</Text>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#1A9E75"}}>
                {item.amount} 
                </Text>
              </View>

              {/* Due Date */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                
                <Text style={{ fontSize: 14, color: "#888", flex: 1 }}>
                  Due Date: {item.dueDate}
                </Text>
        
                <LinearGradient 
                colors={["#E44E2D", "#E4852E"]} 
                start={{ x: 0, y: 0 }} 
                end={{ x: 1, y: 0 }} 
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
              <View style={{height: 1, backgroundColor: "#ddd", marginVertical: 8 }} />

              {/* Challan ID & Buttons */}
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <View style={{backgroundColor: "#F5F5F5", paddingVertical: 8, paddingHorizontal: 10, borderRadius: 10}}>
                  <Text style={{fontSize: 11, color: "#393939"}}>{item.challanId}</Text>
                </View>

                <View style={{flexDirection: "row", alignItems: "center"}}>
                  <TouchableOpacity style={{flexDirection: "row", alignItems: "center", marginRight: 10}}>
                    <Text style={{color: "#1A9E75", fontSize: 12}}>More Info</Text>
                    <Ionicons name="chevron-down" size={14} color="#1A9E75" />
                  </TouchableOpacity>

                  <TouchableOpacity style={{backgroundColor: "#1A9E75", paddingVertical: 5, paddingHorizontal: 11, borderRadius: 8}}>
                    <Text style={{color: "#F0FFFA", fontFamily:"Medium", fontSize: 10}}>Pay Now</Text>
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




























import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView, Modal, FlatList,Image,StyleSheet,Share,StatusBar   } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { LinearGradient } from 'expo-linear-gradient';
import * as Font from "expo-font";
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
  const [expanded, setExpanded] = useState(null);
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
  <TouchableOpacity onPress={() => navigation.goBack()} style={{ position: "absolute", left: 17 }}>
    <Ionicons name="arrow-back" size={24} color="white" />
  </TouchableOpacity>
  <Text 
    style={{ 
      fontSize: 16, 
      fontWeight: "700", 
      color: "#FFFBFB", 
      fontFamily: 'Poppins',
      marginLeft: 56, 
    }}
  >
    Pay Challan
  </Text>
</View>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 15, marginTop: 17, elevation: 2, marginHorizontal: 20,paddingBottom: 20,paddingTop: 11 }}>
        <Text style={{ fontSize: 16, fontWeight: "medium", marginBottom: 5,fontFamily: 'Poppins' }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8,fontWeight: "Normal",lineHeight:20  }}>Enter your vehicle number and check {"\n"}your challans</Text>
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
        backgroundColor: "#E6F9F0",
        borderWidth: 1.5,
        borderColor: isValid ? "#1A9E75" : "red",
        borderColor: "#1A9E75",
        borderRadius: 8,
        paddingHorizontal: 8,
        color: "#1A9E75",
        fontWeight: "bold",
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
      <Text style={{ color: "white", fontWeight: "bold" }}>Check</Text>
    </TouchableOpacity>
  </View>
   {/* Error Message (Only show if there's an error) */}
   {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5,alignSelf: "flex-start" }}>
            {errorMessage}
          </Text>
        )}
</View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Normal", marginBottom: 8,marginTop: -5 }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        {/* Step 1 */}
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
    {/* Text (Number Plate Content) */}
    <SvgText x="90" y="55" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Enter your {"\n"} vehicle number</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        {/* Step 2 */}
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
    {/* Document Shape with Right-Bottom Cut */}
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />
    {/* Folded Corner */}
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    {/* Text Lines */}
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    {/* Challan Label */}
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Check Your {"\n"} Challans</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        {/* Step 3 */}
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
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 3</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14 }}>Make {"\n"} Payment</Text>
        </View>
      </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, marginTop: 3, elevation: 2,width: "90%",marginHorizontal: 20 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8 }}>Traffic Fines</Text>{/* ,color:"#393939"*/}
        <Text style={{fontSize: 14,
                      color: "#AFAFAF",
                      marginBottom: 8,marginTop: -3}}>Select your state to see traffic fines</Text>
        
      {/* Button to Open Modal */}
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
      fontFamily: "Arial",
      
    }}>
      Invite your friends to make hassle-free
    </Text>
  </View>

      {/* Main Content - Text and Image */}
      <View style={{flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",}}>
        {/* Left Section - Text Content */}
        <View style={{flex: 1,
                      paddingRight: 10,}}>
          <Text style={{color: "white",
    fontSize: 18,
    fontFamily:"Poppins",
    fontWeight: "bold",
    marginBottom: 5,marginLeft:15}}>Challan Payments</Text>
          <Text style={{color: "white",
    fontSize: 10,
    marginBottom: 10,
    marginLeft:15}}>Pay using UPI, Debit & Credit Cards</Text>

          {/* Invite Now Button */}
          <TouchableOpacity style={{backgroundColor: "white",
                                    paddingVertical: 6,
                                    paddingHorizontal: 10,
                                    borderRadius: 15,
                                    alignItems: "center",
                                    alignSelf: "flex-start",marginLeft:15}} 
                                    onPress={shareReferral}>
          <Text style={{alignItems: "flex-end",fontSize:10,fontWeight:"bold",fontFamily:"Poppins"}}>Invite Now</Text>
        </TouchableOpacity>
        </View>

        {/* Right Section - Image */}
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
        <Text style={{fontSize: 18, fontWeight: "bold", color: "#333", marginBottom: 10}}>FAQs</Text>
        {faqs.map((faq, index) => (
          <View key={index} style={{backgroundColor: "#FFFFFF", borderRadius: 10, marginBottom: 10, padding: 10, elevation: 2}}>
            <TouchableOpacity style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center", paddingVertical: 10 }} onPress={() => toggleFAQ(index)}>
              <Text style={{fontSize: 15, fontWeight: "600", color: "#333", fontFamily: "Calibri",lineHeight:20}}>{faq.question}</Text>
              <Ionicons name={faq.collapsed ? "chevron-down" : "chevron-up"} size={20} color="#333" />
            </TouchableOpacity>
            <Collapsible collapsed={faq.collapsed}>
              <Text style={{fontSize: 16, color: "#A0A0A0", paddingTop: 4, paddingHorizontal: 5,lineHeight: 18,fontFamily: "Poppins-Regular"}}>{faq.answer}</Text>
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
    { id: "1", title: "Carrying Excess \nLuggage", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs. 1500" },
  { id: "2", title: "Driving Without a \nNumber Plate", amount: "₹ 500",details:"	For first offence Rs. 500, For repeat offence Rs. 1500" },
  { id: "3", title: "Parking in \nNo-parking Zone", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs.1500" },
  { id: "4", title: "Failure to Intimate Change\nof the Address \nof Vehicle Owner", amount: "₹ 500",details:"For the first offence Rs. 500, For repeat offence Rs. 1500" },
  { id: "5", title: "Driving Without a \nSeatbelt", amount: "₹ 1000" },
  { id: "6", title: "Triple Riding on \nTwo-vehicle", amount: "₹ 1000" },
  { id: "10", title: "Driving When Mentally \nor Physically Unfit to Drive", amount: "₹ 1000",details:"For first offence Rs. 1000, For repeat offence Rs. 2000" },
   
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
        <Text style={{ fontSize: 16, fontWeight: "700", color: "#FFFBFB", marginLeft: 12,fontFamily:"Poppins" }}>
          Traffic Fines
        </Text>
      </View>
      
      <ScrollView contentContainerStyle={{ flexGrow: 1, paddingBottom: 20 }} nestedScrollEnabled={true}>
    {/* Scroll Select your State in Fines */}
    <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 10,
          
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#EEEEEE",
          borderWidth:1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          width: "90%",
          marginLeft:20,
          marginTop:20,
        }}
      >
        
        <Text style={{ color:"#AFAFAF", fontSize: 14,marginLeft:10,fontFamily:"Poppins" }}>
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
          padding: 9, 
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
      fontWeight: "bold",

      color: "#333",
      fontFamily: "Poppins_600SemiBold",
      
    }}>
      Traffic fines in Maharashtra 
    </Text>
    
    <Text style={{
      fontSize: 8, 
      color: "#666",
      marginTop: 5,
      fontFamily: "Poppins_400Regular",
    }}>
      List of traffic fines as per Motor Vehicle (Amendment) Act
    </Text>
  </View>
</View>


<View style={{ flex: 1, backgroundColor: "#FFFFFF", padding: 10 }}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        contentContainerStyle={{ paddingBottom: 20 }}
        nestedScrollEnabled={true}
        scrollEnabled={false}
        renderItem={({ item }) => {
          const isExpanded = expandedIds.includes(item.id);
          return (
            
            <View
              style={{
                backgroundColor: "#FFFFFF",
                borderRadius: 15,
                marginBottom: 10,
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
              <TouchableOpacity
                onPress={() => toggleExpand(item.id)}
                
                style={{
                  flexDirection: "row",
                  alignItems: "center",
                  justifyContent: "space-between",
                  position: "relative",
                   
                }}
              >
                <Text
                  style={{
                    fontSize: 14,
                    color: "#393939",
                    flex: 1,
                    fontFamily:"Poppins",
                    textAlign: "left",
                    lineHeight: 20,
                    justifyContent: "center", 
                   
                  }}
                >
                  {item.title}
                </Text>

                 
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

                    <View style={{ flex: 1, alignItems: "center",justifyContent: "center", }}>
                        <Text
                          style={{
                            fontSize: 14,
                            fontWeight: "600",
                            color: "#1A9E75",
                            textAlign: "center",
                            marginLeft:66,
                          }}
                        >
                          {item.amount}
                        </Text>
                      </View>
                <View style={{ flex: 0.2,alignItems: "flex-end"}}>
                {item.details && (
                <Ionicons
                  name={isExpanded ? "chevron-up" : "chevron-down"}
                  size={20} color="#393939"/>
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
                  <Text style={{ fontSize: 15, color: "#A0A0A0",marginLeft:15 }}>
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
          <Text style={{ fontSize: 18, fontWeight: "700", color: "white", marginLeft: 16 }}>Challan Details</Text>
        </View>
  
        
        <View style={{ flexDirection: "row", justifyContent: "space-around", paddingHorizontal:18,marginTop: 10,borderBottomWidth: 1, borderColor: "#ddd",marginLeft:10,marginRight:10 }}>
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Pending" ? "#1A9E75" : "#666" }}>Pending</Text>
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
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Paid" ? "#1A9E75" : "#666" }}>Paid</Text>
          </TouchableOpacity>
          
        </View>
  
      
        {activeTab === "Pending" ? (
          <FlatList
            data={challans}
            keyExtractor={(item) => item.id}
            renderItem={({ item }) => (
              <View style={{backgroundColor: "white", margin: 10, padding: 15, borderRadius: 15, elevation: 2}}>
      
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#000"}}>{item.vehicleNumber}</Text>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#1A9E75"}}>
                {item.amount} 
                </Text>
              </View>

              {/* Due Date */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                
                <Text style={{ fontSize: 14, color: "#888", flex: 1 }}>
                  Due Date: {item.dueDate}
                </Text>
        
                <LinearGradient 
                colors={["#E44E2D", "#E4852E"]} 
                start={{ x: 0, y: 0 }} 
                end={{ x: 1, y: 0 }} 
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
              <View style={{height: 1, backgroundColor: "#ddd", marginVertical: 8 }} />

              {/* Challan ID & Buttons */}
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <View style={{backgroundColor: "#F5F5F5", paddingVertical: 8, paddingHorizontal: 10, borderRadius: 10}}>
                  <Text style={{fontSize: 11, color: "#393939"}}>{item.challanId}</Text>
                </View>

                <View style={{flexDirection: "row", alignItems: "center"}}>
                  <TouchableOpacity style={{flexDirection: "row", alignItems: "center", marginRight: 10}}>
                    <Text style={{color: "#1A9E75", fontSize: 12}}>More Info</Text>
                    <Ionicons name="chevron-down" size={14} color="#1A9E75" />
                  </TouchableOpacity>

                  <TouchableOpacity style={{backgroundColor: "#1A9E75", paddingVertical: 5, paddingHorizontal: 11, borderRadius: 8}}>
                    <Text style={{color: "#F0FFFA", fontFamily:"Medium", fontSize: 10}}>Pay Now</Text>
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


































import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView, Modal, FlatList,Image,StyleSheet,Share,StatusBar   } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { LinearGradient } from 'expo-linear-gradient';
import * as Font from "expo-font";
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
  const [expanded, setExpanded] = useState(null);
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
  <TouchableOpacity onPress={() => navigation.goBack()} style={{ position: "absolute", left: 17 }}>
    <Ionicons name="arrow-back" size={24} color="white" />
  </TouchableOpacity>
  <Text 
    style={{ 
      fontSize: 16, 
      fontWeight: "700", 
      color: "#FFFBFB", 
      fontFamily: 'Poppins',
      marginLeft: 56, 
    }}
  >
    Pay Challan
  </Text>
</View>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 15, marginTop: 17, elevation: 2, marginHorizontal: 20,paddingBottom: 20,paddingTop: 11 }}>
        <Text style={{ fontSize: 16, fontWeight: "medium", marginBottom: 5,fontFamily: 'Poppins' }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8,fontWeight: "Normal",lineHeight:20  }}>Enter your vehicle number and check {"\n"}your challans</Text>
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
        backgroundColor: "#E6F9F0",
        borderWidth: 1.5,
        borderColor: isValid ? "#1A9E75" : "red",
        borderColor: "#1A9E75",
        borderRadius: 8,
        paddingHorizontal: 8,
        color: "#1A9E75",
        fontWeight: "bold",
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
      <Text style={{ color: "white", fontWeight: "bold" }}>Check</Text>
    </TouchableOpacity>
  </View>
   {/* Error Message (Only show if there's an error) */}
   {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5,alignSelf: "flex-start" }}>
            {errorMessage}
          </Text>
        )}
</View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Normal", marginBottom: 8,marginTop: -5 }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        {/* Step 1 */}
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
    {/* Text (Number Plate Content) */}
    <SvgText x="90" y="55" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Enter your {"\n"} vehicle number</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        {/* Step 2 */}
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
    {/* Document Shape with Right-Bottom Cut */}
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />
    {/* Folded Corner */}
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    {/* Text Lines */}
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    {/* Challan Label */}
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Check Your {"\n"} Challans</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        {/* Step 3 */}
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
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 3</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14 }}>Make {"\n"} Payment</Text>
        </View>
      </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, marginTop: 3, elevation: 2,width: "90%",marginHorizontal: 20 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8 }}>Traffic Fines</Text>{/* ,color:"#393939"*/}
        <Text style={{fontSize: 14,
                      color: "#AFAFAF",
                      marginBottom: 8,marginTop: -3}}>Select your state to see traffic fines</Text>
        
      {/* Button to Open Modal */}
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
      fontFamily: "Arial",
      
    }}>
      Invite your friends to make hassle-free
    </Text>
  </View>

      {/* Main Content - Text and Image */}
      <View style={{flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",}}>
        {/* Left Section - Text Content */}
        <View style={{flex: 1,
                      paddingRight: 10,}}>
          <Text style={{color: "white",
    fontSize: 18,
    fontFamily:"Poppins",
    fontWeight: "bold",
    marginBottom: 5,marginLeft:15}}>Challan Payments</Text>
          <Text style={{color: "white",
    fontSize: 10,
    marginBottom: 10,
    marginLeft:15}}>Pay using UPI, Debit & Credit Cards</Text>

          {/* Invite Now Button */}
          <TouchableOpacity style={{backgroundColor: "white",
                                    paddingVertical: 6,
                                    paddingHorizontal: 10,
                                    borderRadius: 15,
                                    alignItems: "center",
                                    alignSelf: "flex-start",marginLeft:15}} 
                                    onPress={shareReferral}>
          <Text style={{alignItems: "flex-end",fontSize:10,fontWeight:"bold",fontFamily:"Poppins"}}>Invite Now</Text>
        </TouchableOpacity>
        </View>

        {/* Right Section - Image */}
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
        <Text style={{fontSize: 18, fontWeight: "bold", color: "#333", marginBottom: 10}}>FAQs</Text>
        {faqs.map((faq, index) => (
          <View key={index} style={{backgroundColor: "#FFFFFF", borderRadius: 10, marginBottom: 10, padding: 10, elevation: 2}}>
            <TouchableOpacity style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center", paddingVertical: 10 }} onPress={() => toggleFAQ(index)}>
              <Text style={{fontSize: 15, fontWeight: "600", color: "#333", fontFamily: "Calibri",lineHeight:20}}>{faq.question}</Text>
              <Ionicons name={faq.collapsed ? "chevron-down" : "chevron-up"} size={20} color="#333" />
            </TouchableOpacity>
            <Collapsible collapsed={faq.collapsed}>
              <Text style={{fontSize: 16, color: "#A0A0A0", paddingTop: 4, paddingHorizontal: 5,lineHeight: 18,fontFamily: "Poppins-Regular"}}>{faq.answer}</Text>
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
    { id: "1", title: "Carrying Excess \nLuggage", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs. 1500" },
  { id: "2", title: "Driving Without a \nNumber Plate", amount: "₹ 500",details:"	For first offence Rs. 500, For repeat offence Rs. 1500" },
  { id: "3", title: "Parking in \nNo-parking Zone", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs.1500" },
  { id: "4", title: "Failure to Intimate Change\nof the Address \nof Vehicle Owner", amount: "₹ 500",details:"For the first offence Rs. 500, For repeat offence Rs. 1500" },

  { id: "5", title: "Driving Without a \nSeatbelt", amount: "₹ 1000" },
  { id: "6", title: "Triple Riding on \nTwo-vehicle", amount: "₹ 1000" },
 
  { id: "10", title: "Driving When Mentally \nor Physically Unfit to Drive", amount: "₹ 1000",details:"For first offence Rs. 1000, For repeat offence Rs. 2000" },
   
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
        <Text style={{ fontSize: 16, fontWeight: "700", color: "#FFFBFB", marginLeft: 12,fontFamily:"Poppins" }}>
          Traffic Fines
        </Text>
      </View>

    {/* Select your State in Fines */}
    <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 10,
          
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#EEEEEE",
          borderWidth:1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          width: "90%",
          marginLeft:20,
          marginTop:20,
        }}
      >
        <Text style={{ color:"#AFAFAF", fontSize: 14,marginLeft:10,fontFamily:"Poppins" }}>
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
          padding: 9, 
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
      fontWeight: "bold",

      color: "#333",
      fontFamily: "Poppins_600SemiBold",
      
    }}>
      Traffic fines in Maharashtra 
    </Text>
    
    <Text style={{
      fontSize: 8, 
      color: "#666",
      marginTop: 5,
      fontFamily: "Poppins_400Regular",
    }}>
      List of traffic fines as per Motor Vehicle (Amendment) Act
    </Text>
  </View>
</View>


<View style={{ flex: 1, backgroundColor: "#FFFFFF", padding: 10 }}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        contentContainerStyle={{ paddingBottom: 20 }}
        renderItem={({ item }) => {
          const isExpanded = expandedIds.includes(item.id);
          return (
            <View
              style={{
                backgroundColor: "#FFFFFF",
                borderRadius: 15,
                marginBottom: 10,
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
              <TouchableOpacity
                onPress={() => toggleExpand(item.id)}
                
                style={{
                  flexDirection: "row",
                  alignItems: "center",
                  justifyContent: "space-between",
                  position: "relative",
                   
                }}
              >
                <Text
                  style={{
                    fontSize: 14,
                    color: "#393939",
                    flex: 1,
                    fontFamily:"Poppins",
                    textAlign: "left",
                    lineHeight: 20,
                    justifyContent: "center", 
                   
                  }}
                >
                  {item.title}
                </Text>

                 
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

                    <View style={{ flex: 1, alignItems: "center",justifyContent: "center", }}>
                        <Text
                          style={{
                            fontSize: 14,
                            fontWeight: "600",
                            color: "#1A9E75",
                            textAlign: "center",
                            marginLeft:66,
                          }}
                        >
                          {item.amount}
                        </Text>
                      </View>
                <View style={{ flex: 0.2,alignItems: "flex-end"}}>
                {item.details && (
                <Ionicons
                  name={isExpanded ? "chevron-up" : "chevron-down"}
                  size={20} color="#393939"/>
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
                  <Text style={{ fontSize: 15, color: "#A0A0A0",marginLeft:15 }}>
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
          <Text style={{ fontSize: 18, fontWeight: "700", color: "white", marginLeft: 16 }}>Challan Details</Text>
        </View>
  
        
        <View style={{ flexDirection: "row", justifyContent: "space-around", paddingHorizontal:18,marginTop: 10,borderBottomWidth: 1, borderColor: "#ddd",marginLeft:10,marginRight:10 }}>
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Pending" ? "#1A9E75" : "#666" }}>Pending</Text>
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
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Paid" ? "#1A9E75" : "#666" }}>Paid</Text>
          </TouchableOpacity>
          
        </View>
  
      
        {activeTab === "Pending" ? (
          <FlatList
            data={challans}
            keyExtractor={(item) => item.id}
            renderItem={({ item }) => (
              <View style={{backgroundColor: "white", margin: 10, padding: 15, borderRadius: 15, elevation: 2}}>
      
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#000"}}>{item.vehicleNumber}</Text>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#1A9E75"}}>
                {item.amount} 
                </Text>
              </View>

              {/* Due Date */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                
                <Text style={{ fontSize: 14, color: "#888", flex: 1 }}>
                  Due Date: {item.dueDate}
                </Text>
        
                <LinearGradient 
                colors={["#E44E2D", "#E4852E"]} 
                start={{ x: 0, y: 0 }} 
                end={{ x: 1, y: 0 }} 
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
              <View style={{height: 1, backgroundColor: "#ddd", marginVertical: 8 }} />

              {/* Challan ID & Buttons */}
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <View style={{backgroundColor: "#F5F5F5", paddingVertical: 8, paddingHorizontal: 10, borderRadius: 10}}>
                  <Text style={{fontSize: 11, color: "#393939"}}>{item.challanId}</Text>
                </View>

                <View style={{flexDirection: "row", alignItems: "center"}}>
                  <TouchableOpacity style={{flexDirection: "row", alignItems: "center", marginRight: 10}}>
                    <Text style={{color: "#1A9E75", fontSize: 12}}>More Info</Text>
                    <Ionicons name="chevron-down" size={14} color="#1A9E75" />
                  </TouchableOpacity>

                  <TouchableOpacity style={{backgroundColor: "#1A9E75", paddingVertical: 5, paddingHorizontal: 11, borderRadius: 8}}>
                    <Text style={{color: "#F0FFFA", fontFamily:"Medium", fontSize: 10}}>Pay Now</Text>
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




























































import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView, Modal, FlatList,Image,StyleSheet,Share,StatusBar   } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { LinearGradient } from 'expo-linear-gradient';
import * as Font from "expo-font";
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
  const [expanded, setExpanded] = useState(null);
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

  // const toggleExpand = (index) => {
  //   setExpanded(expanded === index ? null : index);
  // };

  

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
  <TouchableOpacity onPress={() => navigation.goBack()} style={{ position: "absolute", left: 17 }}>
    <Ionicons name="arrow-back" size={24} color="white" />
  </TouchableOpacity>
  <Text 
    style={{ 
      fontSize: 16, 
      fontWeight: "700", 
      color: "#FFFBFB", 
      fontFamily: 'Poppins',
      marginLeft: 56, 
    }}
  >
    Pay Challan
  </Text>
</View>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 15, marginTop: 17, elevation: 2, marginHorizontal: 20,paddingBottom: 20,paddingTop: 11 }}>
        <Text style={{ fontSize: 16, fontWeight: "medium", marginBottom: 5,fontFamily: 'Poppins' }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8,fontWeight: "Normal",lineHeight:20  }}>Enter your vehicle number and check {"\n"}your challans</Text>
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
        backgroundColor: "#E6F9F0",
        borderWidth: 1.5,
        borderColor: isValid ? "#1A9E75" : "red",
        borderColor: "#1A9E75",
        borderRadius: 8,
        paddingHorizontal: 8,
        color: "#1A9E75",
        fontWeight: "bold",
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
      <Text style={{ color: "white", fontWeight: "bold" }}>Check</Text>
    </TouchableOpacity>
  </View>
   {/* Error Message (Only show if there's an error) */}
   {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5,alignSelf: "flex-start" }}>
            {errorMessage}
          </Text>
        )}
</View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Normal", marginBottom: 8,marginTop: -5 }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        {/* Step 1 */}
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
    {/* Text (Number Plate Content) */}
    <SvgText x="90" y="55" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Enter your {"\n"} vehicle number</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        {/* Step 2 */}
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
    {/* Document Shape with Right-Bottom Cut */}
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />
    {/* Folded Corner */}
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    {/* Text Lines */}
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    {/* Challan Label */}
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Check Your {"\n"} Challans</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        {/* Step 3 */}
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
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 3</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14 }}>Make {"\n"} Payment</Text>
        </View>
      </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, marginTop: 3, elevation: 2,width: "90%",marginHorizontal: 20 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8 }}>Traffic Fines</Text>{/* ,color:"#393939"*/}
        <Text style={{fontSize: 14,
                      color: "#AFAFAF",
                      marginBottom: 8,marginTop: -3}}>Select your state to see traffic fines</Text>
        
      {/* Button to Open Modal */}
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
      fontFamily: "Arial",
      
    }}>
      Invite your friends to make hassle-free
    </Text>
  </View>

      {/* Main Content - Text and Image */}
      <View style={{flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",}}>
        {/* Left Section - Text Content */}
        <View style={{flex: 1,
                      paddingRight: 10,}}>
          <Text style={{color: "white",
    fontSize: 18,
    fontFamily:"Poppins",
    fontWeight: "bold",
    marginBottom: 5,marginLeft:15}}>Challan Payments</Text>
          <Text style={{color: "white",
    fontSize: 10,
    marginBottom: 10,
    marginLeft:15}}>Pay using UPI, Debit & Credit Cards</Text>

          {/* Invite Now Button */}
          <TouchableOpacity style={{backgroundColor: "white",
                                    paddingVertical: 6,
                                    paddingHorizontal: 10,
                                    borderRadius: 15,
                                    alignItems: "center",
                                    alignSelf: "flex-start",marginLeft:15}} 
                                    onPress={shareReferral}>
          <Text style={{alignItems: "flex-end",fontSize:10,fontWeight:"bold",fontFamily:"Poppins"}}>Invite Now</Text>
        </TouchableOpacity>
        </View>

        {/* Right Section - Image */}
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
        <Text style={{fontSize: 18, fontWeight: "bold", color: "#333", marginBottom: 10}}>FAQs</Text>
        {faqs.map((faq, index) => (
          <View key={index} style={{backgroundColor: "#FFFFFF", borderRadius: 10, marginBottom: 10, padding: 10, elevation: 2}}>
            <TouchableOpacity style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center", paddingVertical: 10 }} onPress={() => toggleFAQ(index)}>
              <Text style={{fontSize: 15, fontWeight: "600", color: "#333", fontFamily: "Calibri",lineHeight:20}}>{faq.question}</Text>
              <Ionicons name={faq.collapsed ? "chevron-down" : "chevron-up"} size={20} color="#333" />
            </TouchableOpacity>
            <Collapsible collapsed={faq.collapsed}>
              <Text style={{fontSize: 16, color: "#A0A0A0", paddingTop: 4, paddingHorizontal: 5,lineHeight: 18,fontFamily: "Poppins-Regular"}}>{faq.answer}</Text>
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
    { id: "1", title: "Carrying Excess \nLuggage", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs. 1500" },
  { id: "2", title: "Driving Without a \nNumber Plate", amount: "₹ 500",details:"	For first offence Rs. 500, For repeat offence Rs. 1500" },
  { id: "3", title: "Parking in \nNo-parking Zone", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs.1500" },
  { id: "4", title: "Failure to Intimate Change\nof the Address \nof Vehicle Owner", amount: "₹ 500",details:"For the first offence Rs. 500, For repeat offence Rs. 1500" },

  { id: "5", title: "Driving Without a \nSeatbelt", amount: "₹ 1000" },
  { id: "6", title: "Triple Riding on \nTwo-vehicle", amount: "₹ 1000" },
  { id: "7", title: "Driving Without \nHelmet", amount: "₹ 1000" },
  { id: "8", title: "Over-speeding", amount: "₹ 1000",details:"For Light motor vehicle (LMV) Rs. 1000, For Medium passenger goods vehicle Rs. 2000" },
  { id: "9", title: "Violation of Road \nRegulations", amount: "₹ 1000" },
  { id: "10", title: "Driving When Mentally \nor Physically Unfit to Drive", amount: "₹ 1000",details:"For first offence Rs. 1000, For repeat offence Rs. 2000" },
   { id: "11", title: "Driving Without \nInsurance", amount: "₹ 2000" },
  { id: "12", title: "Overloading", amount: "₹ 2000" },
  { id: "13", title: "Driving Uninsured Vehicle", amount: "₹ 2000" },
   { id: "14", title: "Driving Vehicle \nWithout Registration", amount: "₹ 2000" },

    { id: "15", title: "Dangerous/Rash \nDriving", amount: "₹ 5000",details:"For first offence Rs. 5000, For repeat offence Rs. 10000" },
  { id: "16", title: "Disobey of Traffic \nSignals", amount: "₹ 5000",details:"For first offence Rs. 5000, For repeat offence Rs. 10000" },
  { id: "17", title: "Using a Mobile \nPhone While Driving", amount: "₹ 5000",details:"For the first offence Rs. 5000, For repeat offence Rs. 10000" },
  { id: "18", title: "Racing", amount: "₹ 5000",details:"For the first offence Rs. 5000, For repeat offence Rs. 10000" },
  { id: "19", title: "Driving Without a \nValid Driving Licence", amount: "₹ 5000" },
  { id: "20", title: "Drunk Driving", amount: "₹ 10,000" },
  { id: "21", title: "Carrying Explosive/\nInflammable Substances", amount: "₹ 10,000" },
  { id: "22", title: "Not Giving Passage \nto Emergency Vehicles", amount: "₹ 10,000" },
  { id: "23", title: "Disqualified Person \nDriving a Vehicle", amount: "₹ 10,000" },
   { id: "24", title: "Minor Driving \nVehicle", amount: "₹ 25,000" },
  // { id: "25", title: "Driving a Vehicle \nRegistered in Another \nState for More Than 12 Months", amount: "₹ 500" },
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
        <Text style={{ fontSize: 16, fontWeight: "700", color: "#FFFBFB", marginLeft: 12,fontFamily:"Poppins" }}>
          Traffic Fines
        </Text>
      </View>

    {/* Select your State in Fines */}
    <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 10,
          
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#EEEEEE",
          borderWidth:1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          width: "90%",
          marginLeft:20,
          marginTop:20,
        }}
      >
        <Text style={{ color:"#AFAFAF", fontSize: 14,marginLeft:10,fontFamily:"Poppins" }}>
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
          padding: 9, 
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
      fontWeight: "bold",

      color: "#333",
      fontFamily: "Poppins_600SemiBold",
      
    }}>
      Traffic fines in Maharashtra 
    </Text>
    
    <Text style={{
      fontSize: 8, 
      color: "#666",
      marginTop: 5,
      fontFamily: "Poppins_400Regular",
    }}>
      List of traffic fines as per Motor Vehicle (Amendment) Act
    </Text>
  </View>
</View>


<View style={{ flex: 1, backgroundColor: "#FFFFFF", padding: 10 }}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        contentContainerStyle={{ paddingBottom: 20 }}
        renderItem={({ item }) => {
          const isExpanded = expandedIds.includes(item.id);
          return (
            <View
              style={{
                backgroundColor: "#FFFFFF",
                borderRadius: 15,
                marginBottom: 10,
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
              <TouchableOpacity
                onPress={() => toggleExpand(item.id)}
                
                style={{
                  flexDirection: "row",
                  alignItems: "center",
                  justifyContent: "space-between",
                  position: "relative",
                   
                }}
              >
                <Text
                  style={{
                    fontSize: 14,
                    color: "#393939",
                    flex: 1,
                    fontFamily:"Poppins",
                    textAlign: "left",
                    lineHeight: 20,
                    justifyContent: "center", 
                   
                  }}
                >
                  {item.title}
                </Text>

                 
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

                    <View style={{ flex: 1, alignItems: "center",justifyContent: "center", }}>
                        <Text
                          style={{
                            fontSize: 14,
                            fontWeight: "600",
                            color: "#1A9E75",
                            textAlign: "center",
                            marginLeft:66,
                          }}
                        >
                          {item.amount}
                        </Text>
                      </View>
                <View style={{ flex: 0.2,alignItems: "flex-end"}}>
                {item.details && (
                <Ionicons
                  name={isExpanded ? "chevron-up" : "chevron-down"}
                  size={20} color="#393939"/>
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
                  <Text style={{ fontSize: 15, color: "#A0A0A0",marginLeft:15 }}>
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
          <Text style={{ fontSize: 18, fontWeight: "700", color: "white", marginLeft: 16 }}>Challan Details</Text>
        </View>
  
        
        <View style={{ flexDirection: "row", justifyContent: "space-around", paddingHorizontal:18,marginTop: 10,borderBottomWidth: 1, borderColor: "#ddd",marginLeft:10,marginRight:10 }}>
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Pending" ? "#1A9E75" : "#666" }}>Pending</Text>
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
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Paid" ? "#1A9E75" : "#666" }}>Paid</Text>
          </TouchableOpacity>
          
        </View>
  
      
        {activeTab === "Pending" ? (
          <FlatList
            data={challans}
            keyExtractor={(item) => item.id}
            renderItem={({ item }) => (
              <View style={{backgroundColor: "white", margin: 10, padding: 15, borderRadius: 15, elevation: 2}}>
      
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#000"}}>{item.vehicleNumber}</Text>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#1A9E75"}}>
                {item.amount} 
                </Text>
              </View>

              {/* Due Date */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                
                <Text style={{ fontSize: 14, color: "#888", flex: 1 }}>
                  Due Date: {item.dueDate}
                </Text>
        
                <LinearGradient 
                colors={["#E44E2D", "#E4852E"]} 
                start={{ x: 0, y: 0 }} 
                end={{ x: 1, y: 0 }} 
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
              <View style={{height: 1, backgroundColor: "#ddd", marginVertical: 8 }} />

              {/* Challan ID & Buttons */}
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <View style={{backgroundColor: "#F5F5F5", paddingVertical: 8, paddingHorizontal: 10, borderRadius: 10}}>
                  <Text style={{fontSize: 11, color: "#393939"}}>{item.challanId}</Text>
                </View>

                <View style={{flexDirection: "row", alignItems: "center"}}>
                  <TouchableOpacity style={{flexDirection: "row", alignItems: "center", marginRight: 10}}>
                    <Text style={{color: "#1A9E75", fontSize: 12}}>More Info</Text>
                    <Ionicons name="chevron-down" size={14} color="#1A9E75" />
                  </TouchableOpacity>

                  <TouchableOpacity style={{backgroundColor: "#1A9E75", paddingVertical: 5, paddingHorizontal: 11, borderRadius: 8}}>
                    <Text style={{color: "#F0FFFA", fontFamily:"Medium", fontSize: 10}}>Pay Now</Text>
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





































import React, { useState } from "react";
import { View, Text, FlatList, TouchableOpacity, Modal } from "react-native";
import { Ionicons, MaterialCommunityIcons } from "@expo/vector-icons";

const states = ["Maharashtra", "Karnataka", "Delhi", "Tamil Nadu", "Gujarat", "West Bengal"];
const finesData = [
  { id: "1", title: "Carrying Excess \nLuggage", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs. 1500" },
  { id: "2", title: "Driving Without a \nNumber Plate", amount: "₹ 500",details:"	For first offence Rs. 500, For repeat offence Rs. 1500" },
  { id: "3", title: "Parking in \nNo-parking Zone", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs.1500" },
  { id: "4", title: "Failure to Intimate Change\nof the Address \nof Vehicle Owner", amount: "₹ 500",details:"For the first offence Rs. 500, For repeat offence Rs. 1500" },

  { id: "5", title: "Driving Without a \nSeatbelt", amount: "₹ 1000" },
  { id: "6", title: "Triple Riding on \nTwo-vehicle", amount: "₹ 1000" },
  { id: "7", title: "Driving Without \nHelmet", amount: "₹ 1000" },
  { id: "8", title: "Over-speeding", amount: "₹ 1000",details:"For Light motor vehicle (LMV) Rs. 1000, For Medium passenger goods vehicle Rs. 2000" },
  { id: "9", title: "Violation of Road \nRegulations", amount: "₹ 1000" },
  { id: "10", title: "Driving When Mentally \nor Physically Unfit to Drive", amount: "₹ 1000",details:"For first offence Rs. 1000, For repeat offence Rs. 2000" },
   { id: "11", title: "Driving Without \nInsurance", amount: "₹ 2000" },
  { id: "12", title: "Overloading", amount: "₹ 2000" },
  { id: "13", title: "Driving Uninsured Vehicle", amount: "₹ 2000" },
   { id: "14", title: "Driving Vehicle \nWithout Registration", amount: "₹ 2000" },

    { id: "15", title: "Dangerous/Rash \nDriving", amount: "₹ 5000",details:"For first offence Rs. 5000, For repeat offence Rs. 10000" },
  { id: "16", title: "Disobey of Traffic \nSignals", amount: "₹ 5000",details:"For first offence Rs. 5000, For repeat offence Rs. 10000" },
  { id: "17", title: "Using a Mobile \nPhone While Driving", amount: "₹ 5000",details:"For the first offence Rs. 5000, For repeat offence Rs. 10000" },
  { id: "18", title: "Racing", amount: "₹ 5000",details:"For the first offence Rs. 5000, For repeat offence Rs. 10000" },
  { id: "19", title: "Driving Without a \nValid Driving Licence", amount: "₹ 5000" },
  { id: "20", title: "Drunk Driving", amount: "₹ 10,000" },
  { id: "21", title: "Carrying Explosive/\nInflammable Substances", amount: "₹ 10,000" },
  { id: "22", title: "Not Giving Passage \nto Emergency Vehicles", amount: "₹ 10,000" },
  { id: "23", title: "Disqualified Person \nDriving a Vehicle", amount: "₹ 10,000" },
   { id: "24", title: "Minor Driving \nVehicle", amount: "₹ 25,000" },
  // { id: "25", title: "Driving a Vehicle \nRegistered in Another \nState for More Than 12 Months", amount: "₹ 500" },
];

const StateChallanScreen = ({ navigation }) => {
  const [modalVisible, setModalVisible] = useState(false);
  const [selectedState, setSelectedState] = useState(null);
  const [expandedIds, setExpandedIds] = useState([]);

  const toggleExpand = (id) => {
    setExpandedIds((prev) =>
      prev.includes(id) ? prev.filter((item) => item !== id) : [...prev, id]
    );
  };

  return (
    <View style={{ flex: 1, backgroundColor: "#FFFFFF" }}>
      
      {/* 🔹 Header */}
      <View style={{ backgroundColor: "#1A9E75", height: 88, flexDirection: "row", alignItems: "center", paddingHorizontal: 13, paddingTop: 25 }}>
        <TouchableOpacity onPress={() => navigation.goBack()}>
          <Ionicons name="arrow-back" size={24} color="white" />
        </TouchableOpacity>
        <Text style={{ fontSize: 16, fontWeight: "700", color: "#FFFBFB", marginLeft: 12, fontFamily: "Poppins" }}>
          Traffic Fines
        </Text>
      </View>

      {/* 🔹 Select Your State */}
      <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 10,
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#EEEEEE",
          borderWidth: 1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          width: "90%",
          alignSelf: "center",
          marginTop: 20,
        }}
      >
        <Text style={{ color: "#AFAFAF", fontSize: 14, marginLeft: 10, fontFamily: "Poppins" }}>
          {selectedState || "Select Your State"}
        </Text>
        <Ionicons name="caret-down-sharp" size={20} color="#27A47D" />
      </TouchableOpacity>

      {/* 🔹 State Selection Modal */}
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
            
            {/* 🔸 Header with Close Button */}
            <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
              <Text style={{ fontSize: 18, fontWeight: "bold", color: "#333", marginBottom: 14 }}>Choose State</Text>
              <TouchableOpacity onPress={() => setModalVisible(false)}>
                <Ionicons name="close-circle" size={24} color="green" />
              </TouchableOpacity>
            </View>

            {/* 🔸 State List */}
            <FlatList
              data={states}
              keyExtractor={(item) => item}
              renderItem={({ item }) => (
                <TouchableOpacity
                  style={{ flexDirection: "row", alignItems: "center", paddingVertical: 8 }}
                  onPress={() => {
                    setSelectedState(item);
                    setModalVisible(false);
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

      {/* 🔹 Traffic Fines Header */}
      <View style={{
        flexDirection: "row",
        alignItems: "center",
        backgroundColor: "#F0FFFA",
        padding: 9,
        borderRadius: 12,
        width: "91%",
        alignSelf: "center",
        marginTop: 10,
      }}>
        <MaterialCommunityIcons name="traffic-light" size={44} color="#1A9E75" style={{ marginRight: 8 }} />
        
        {/* 🔹 Text Container */}
        <View style={{ flex: 1 }}>  
          <Text style={{ fontSize: 16, fontWeight: "bold", color: "#333", fontFamily: "Poppins_600SemiBold" }}>
            Traffic fines in {selectedState || "Maharashtra"}
          </Text>
          <Text style={{ fontSize: 12, color: "#666", marginTop: 5, fontFamily: "Poppins_400Regular" }}>
            List of traffic fines as per Motor Vehicle (Amendment) Act
          </Text>
        </View>
      </View>

      {/* 🔹 Traffic Fines List */}
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        contentContainerStyle={{ paddingBottom: 20 }}
        renderItem={({ item }) => {
          const isExpanded = expandedIds.includes(item.id);
          return (
            <View style={{
              backgroundColor: "#FFFFFF",
              borderRadius: 15,
              marginBottom: 10,
              elevation: 3,
              width: "92%",
              alignSelf: "center",
              padding: 15,
              borderColor: "#E4E4E4",
              borderWidth: 1,
            }}>
              <TouchableOpacity
                onPress={() => toggleExpand(item.id)}
                style={{
                  flexDirection: "row",
                  alignItems: "center",
                  justifyContent: "space-between",
                }}
              >
                <Text style={{ fontSize: 14, color: "#393939", flex: 1 }}>{item.title}</Text>
                <Text style={{ fontSize: 14, fontWeight: "600", color: "#1A9E75", marginRight: 15 }}>
                  {item.amount}
                </Text>
                {item.details && (
                  <Ionicons name={isExpanded ? "chevron-up" : "chevron-down"} size={20} color="#393939" />
                )}
              </TouchableOpacity>

              {isExpanded && item.details && (
                <View style={{ backgroundColor: "white", padding: 8, borderRadius: 8, marginTop: 5 }}>
                  <Text style={{ fontSize: 13, color: "#A0A0A0", marginLeft: 5 }}>{item.details}</Text>
                </View>
              )}
            </View>
          );
        }}
      />
    </View>
  );
};

export default StateChallanScreen;




















































import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView, Modal, FlatList,Image,StyleSheet,Share,StatusBar   } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { LinearGradient } from 'expo-linear-gradient';
import * as Font from "expo-font";
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
  const [expanded, setExpanded] = useState(null);
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

  // const toggleExpand = (index) => {
  //   setExpanded(expanded === index ? null : index);
  // };

  

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
  <TouchableOpacity onPress={() => navigation.goBack()} style={{ position: "absolute", left: 17 }}>
    <Ionicons name="arrow-back" size={24} color="white" />
  </TouchableOpacity>
  <Text 
    style={{ 
      fontSize: 16, 
      fontWeight: "700", 
      color: "#FFFBFB", 
      fontFamily: 'Poppins',
      marginLeft: 56, 
    }}
  >
    Pay Challan
  </Text>
</View>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 15, marginTop: 17, elevation: 2, marginHorizontal: 20,paddingBottom: 20,paddingTop: 11 }}>
        <Text style={{ fontSize: 16, fontWeight: "medium", marginBottom: 5,fontFamily: 'Poppins' }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8,fontWeight: "Normal",lineHeight:20  }}>Enter your vehicle number and check {"\n"}your challans</Text>
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
        backgroundColor: "#E6F9F0",
        borderWidth: 1.5,
        borderColor: isValid ? "#1A9E75" : "red",
        borderColor: "#1A9E75",
        borderRadius: 8,
        paddingHorizontal: 8,
        color: "#1A9E75",
        fontWeight: "bold",
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
      <Text style={{ color: "white", fontWeight: "bold" }}>Check</Text>
    </TouchableOpacity>
  </View>
   {/* Error Message (Only show if there's an error) */}
   {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5,alignSelf: "flex-start" }}>
            {errorMessage}
          </Text>
        )}
</View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Normal", marginBottom: 8,marginTop: -5 }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        {/* Step 1 */}
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
    {/* Text (Number Plate Content) */}
    <SvgText x="90" y="55" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Enter your {"\n"} vehicle number</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        {/* Step 2 */}
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
    {/* Document Shape with Right-Bottom Cut */}
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />
    {/* Folded Corner */}
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    {/* Text Lines */}
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    {/* Challan Label */}
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Check Your {"\n"} Challans</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        {/* Step 3 */}
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
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 3</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14 }}>Make {"\n"} Payment</Text>
        </View>
      </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, marginTop: 3, elevation: 2,width: "90%",marginHorizontal: 20 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8 }}>Traffic Fines</Text>{/* ,color:"#393939"*/}
        <Text style={{fontSize: 14,
                      color: "#AFAFAF",
                      marginBottom: 8,marginTop: -3}}>Select your state to see traffic fines</Text>
        
      {/* Button to Open Modal */}
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
      fontFamily: "Arial",
      
    }}>
      Invite your friends to make hassle-free
    </Text>
  </View>

      {/* Main Content - Text and Image */}
      <View style={{flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",}}>
        {/* Left Section - Text Content */}
        <View style={{flex: 1,
                      paddingRight: 10,}}>
          <Text style={{color: "white",
    fontSize: 18,
    fontFamily:"Poppins",
    fontWeight: "bold",
    marginBottom: 5,marginLeft:15}}>Challan Payments</Text>
          <Text style={{color: "white",
    fontSize: 10,
    marginBottom: 10,
    marginLeft:15}}>Pay using UPI, Debit & Credit Cards</Text>

          {/* Invite Now Button */}
          <TouchableOpacity style={{backgroundColor: "white",
                                    paddingVertical: 6,
                                    paddingHorizontal: 10,
                                    borderRadius: 15,
                                    alignItems: "center",
                                    alignSelf: "flex-start",marginLeft:15}} 
                                    onPress={shareReferral}>
          <Text style={{alignItems: "flex-end",fontSize:10,fontWeight:"bold",fontFamily:"Poppins"}}>Invite Now</Text>
        </TouchableOpacity>
        </View>

        {/* Right Section - Image */}
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
        <Text style={{fontSize: 18, fontWeight: "bold", color: "#333", marginBottom: 10}}>FAQs</Text>
        {faqs.map((faq, index) => (
          <View key={index} style={{backgroundColor: "#FFFFFF", borderRadius: 10, marginBottom: 10, padding: 10, elevation: 2}}>
            <TouchableOpacity style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center", paddingVertical: 10 }} onPress={() => toggleFAQ(index)}>
              <Text style={{fontSize: 15, fontWeight: "600", color: "#333", fontFamily: "Calibri",lineHeight:20}}>{faq.question}</Text>
              <Ionicons name={faq.collapsed ? "chevron-down" : "chevron-up"} size={20} color="#333" />
            </TouchableOpacity>
            <Collapsible collapsed={faq.collapsed}>
              <Text style={{fontSize: 16, color: "#A0A0A0", paddingTop: 4, paddingHorizontal: 5,lineHeight: 18,fontFamily: "Poppins-Regular"}}>{faq.answer}</Text>
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
    { id: "2", title: "Driving Without a \nNumber Plate", amount: "₹ 500",details:"	For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "3", title: "Parking in \nNo-parking Zone", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs.1500" },
    { id: "4", title: "Failure to Intimate Change\nof the Address \nof Vehicle Owner", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "5", title: "Driving Without a \nSeatbelt", amount: "₹ 1000" },
    { id: "6", title: "Triple Riding on \nTwo-vehicle", amount: "₹ 1000" },
    { id: "7", title: "Driving Without Helmet", amount: "₹ 1000" },
    { id: "8", title: "Over-speeding", amount: "₹ 1000",details:"For Light motor vehicle (LMV) Rs. 1000, For Medium passenger goods vehicle Rs. 2000" },
    { id: "9", title: "Violation of Road \nRegulations", amount: "₹ 1000" },
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
        <Text style={{ fontSize: 16, fontWeight: "700", color: "#FFFBFB", marginLeft: 12,fontFamily:"Poppins" }}>
          Traffic Fines
        </Text>
      </View>

    {/* Select your State in Fines */}
    <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 10,
          
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#EEEEEE",
          borderWidth:1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          width: "90%",
          marginLeft:20,
          marginTop:20,
        }}
      >
        <Text style={{ color:"#AFAFAF", fontSize: 14,marginLeft:10,fontFamily:"Poppins" }}>
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
          padding: 9, 
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
      fontWeight: "bold",

      color: "#333",
      fontFamily: "Poppins_600SemiBold",
      
    }}>
      Traffic fines in Maharashtra 
    </Text>
    
    <Text style={{
      fontSize: 8, 
      color: "#666",
      marginTop: 5,
      fontFamily: "Poppins_400Regular",
    }}>
      List of traffic fines as per Motor Vehicle (Amendment) Act
    </Text>
  </View>
</View>


<View style={{ flex: 1, backgroundColor: "#FFFFFF", padding: 10 }}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        contentContainerStyle={{ paddingBottom: 20 }}
        renderItem={({ item }) => {
          const isExpanded = expandedIds.includes(item.id);
          return (
            <View
              style={{
                backgroundColor: "#FFFFFF",
                borderRadius: 15,
                marginBottom: 10,
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
              <TouchableOpacity
                onPress={() => toggleExpand(item.id)}
                
                style={{
                  flexDirection: "row",
                  alignItems: "center",
                  justifyContent: "space-between",
                  position: "relative",
                   
                }}
              >
                <Text
                  style={{
                    fontSize: 14,
                    color: "#393939",
                    flex: 1,
                    fontFamily:"Poppins",
                    textAlign: "left",
                    lineHeight: 20,
                    justifyContent: "center", 
                   
                  }}
                >
                  {item.title}
                </Text>

                 
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

                    <View style={{ flex: 1, alignItems: "center",justifyContent: "center", }}>
                        <Text
                          style={{
                            fontSize: 14,
                            fontWeight: "600",
                            color: "#1A9E75",
                            textAlign: "center",
                            marginLeft:66,
                          }}
                        >
                          {item.amount}
                        </Text>
                      </View>
                <View style={{ flex: 0.2,alignItems: "flex-end"}}>
                {item.details && (
                <Ionicons
                  name={isExpanded ? "chevron-up" : "chevron-down"}
                  size={20} color="#393939"/>
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
                  <Text style={{ fontSize: 15, color: "#A0A0A0",marginLeft:15 }}>
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
          <Text style={{ fontSize: 18, fontWeight: "700", color: "white", marginLeft: 16 }}>Challan Details</Text>
        </View>
  
        
        <View style={{ flexDirection: "row", justifyContent: "space-around", paddingHorizontal:18,marginTop: 10,borderBottomWidth: 1, borderColor: "#ddd",marginLeft:10,marginRight:10 }}>
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Pending" ? "#1A9E75" : "#666" }}>Pending</Text>
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
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Paid" ? "#1A9E75" : "#666" }}>Paid</Text>
          </TouchableOpacity>
          
        </View>
  
      
        {activeTab === "Pending" ? (
          <FlatList
            data={challans}
            keyExtractor={(item) => item.id}
            renderItem={({ item }) => (
              <View style={{backgroundColor: "white", margin: 10, padding: 15, borderRadius: 15, elevation: 2}}>
      
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#000"}}>{item.vehicleNumber}</Text>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#1A9E75"}}>
                {item.amount} 
                </Text>
              </View>

              {/* Due Date */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                
                <Text style={{ fontSize: 14, color: "#888", flex: 1 }}>
                  Due Date: {item.dueDate}
                </Text>
        
                <LinearGradient 
                colors={["#E44E2D", "#E4852E"]} 
                start={{ x: 0, y: 0 }} 
                end={{ x: 1, y: 0 }} 
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
              <View style={{height: 1, backgroundColor: "#ddd", marginVertical: 8 }} />

              {/* Challan ID & Buttons */}
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <View style={{backgroundColor: "#F5F5F5", paddingVertical: 8, paddingHorizontal: 10, borderRadius: 10}}>
                  <Text style={{fontSize: 11, color: "#393939"}}>{item.challanId}</Text>
                </View>

                <View style={{flexDirection: "row", alignItems: "center"}}>
                  <TouchableOpacity style={{flexDirection: "row", alignItems: "center", marginRight: 10}}>
                    <Text style={{color: "#1A9E75", fontSize: 12}}>More Info</Text>
                    <Ionicons name="chevron-down" size={14} color="#1A9E75" />
                  </TouchableOpacity>

                  <TouchableOpacity style={{backgroundColor: "#1A9E75", paddingVertical: 5, paddingHorizontal: 11, borderRadius: 8}}>
                    <Text style={{color: "#F0FFFA", fontFamily:"Medium", fontSize: 10}}>Pay Now</Text>
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


























import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView, Modal, FlatList,Image,StyleSheet,Share,StatusBar   } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { LinearGradient } from 'expo-linear-gradient';
import * as Font from "expo-font";
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
  const [expanded, setExpanded] = useState(null);
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

  // const toggleExpand = (index) => {
  //   setExpanded(expanded === index ? null : index);
  // };

  

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
  <TouchableOpacity onPress={() => navigation.goBack()} style={{ position: "absolute", left: 17 }}>
    <Ionicons name="arrow-back" size={24} color="white" />
  </TouchableOpacity>
  <Text 
    style={{ 
      fontSize: 16, 
      fontWeight: "700", 
      color: "#FFFBFB", 
      fontFamily: 'Poppins',
      marginLeft: 56, 
    }}
  >
    Pay Challan
  </Text>
</View>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 15, marginTop: 17, elevation: 2, marginHorizontal: 20,paddingBottom: 20,paddingTop: 11 }}>
        <Text style={{ fontSize: 16, fontWeight: "medium", marginBottom: 5,fontFamily: 'Poppins' }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8,fontWeight: "Normal",lineHeight:20  }}>Enter your vehicle number and check {"\n"}your challans</Text>
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
        backgroundColor: "#E6F9F0",
        borderWidth: 1.5,
        borderColor: isValid ? "#1A9E75" : "red",
        borderColor: "#1A9E75",
        borderRadius: 8,
        paddingHorizontal: 8,
        color: "#1A9E75",
        fontWeight: "bold",
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
      <Text style={{ color: "white", fontWeight: "bold" }}>Check</Text>
    </TouchableOpacity>
  </View>
   {/* Error Message (Only show if there's an error) */}
   {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5,alignSelf: "flex-start" }}>
            {errorMessage}
          </Text>
        )}
</View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Normal", marginBottom: 8,marginTop: -5 }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        {/* Step 1 */}
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
    {/* Text (Number Plate Content) */}
    <SvgText x="90" y="55" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Enter your {"\n"} vehicle number</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        {/* Step 2 */}
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
    {/* Document Shape with Right-Bottom Cut */}
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />
    {/* Folded Corner */}
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    {/* Text Lines */}
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    {/* Challan Label */}
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Check Your {"\n"} Challans</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        {/* Step 3 */}
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
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 3</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14 }}>Make {"\n"} Payment</Text>
        </View>
      </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, marginTop: 3, elevation: 2,width: "90%",marginHorizontal: 20 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8 }}>Traffic Fines</Text>{/* ,color:"#393939"*/}
        <Text style={{fontSize: 14,
                      color: "#AFAFAF",
                      marginBottom: 8,marginTop: -3}}>Select your state to see traffic fines</Text>
        
      {/* Button to Open Modal */}
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
      fontFamily: "Arial",
      
    }}>
      Invite your friends to make hassle-free
    </Text>
  </View>

      {/* Main Content - Text and Image */}
      <View style={{flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",}}>
        {/* Left Section - Text Content */}
        <View style={{flex: 1,
                      paddingRight: 10,}}>
          <Text style={{color: "white",
    fontSize: 18,
    fontFamily:"Poppins",
    fontWeight: "bold",
    marginBottom: 5,marginLeft:15}}>Challan Payments</Text>
          <Text style={{color: "white",
    fontSize: 10,
    marginBottom: 10,
    marginLeft:15}}>Pay using UPI, Debit & Credit Cards</Text>

          {/* Invite Now Button */}
          <TouchableOpacity style={{backgroundColor: "white",
                                    paddingVertical: 6,
                                    paddingHorizontal: 10,
                                    borderRadius: 15,
                                    alignItems: "center",
                                    alignSelf: "flex-start",marginLeft:15}} 
                                    onPress={shareReferral}>
          <Text style={{alignItems: "flex-end",fontSize:10,fontWeight:"bold",fontFamily:"Poppins"}}>Invite Now</Text>
        </TouchableOpacity>
        </View>

        {/* Right Section - Image */}
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
        <Text style={{fontSize: 18, fontWeight: "bold", color: "#333", marginBottom: 10}}>FAQs</Text>
        {faqs.map((faq, index) => (
          <View key={index} style={{backgroundColor: "#FFFFFF", borderRadius: 10, marginBottom: 10, padding: 10, elevation: 2}}>
            <TouchableOpacity style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center", paddingVertical: 10 }} onPress={() => toggleFAQ(index)}>
              <Text style={{fontSize: 15, fontWeight: "600", color: "#333", fontFamily: "Calibri",lineHeight:20}}>{faq.question}</Text>
              <Ionicons name={faq.collapsed ? "chevron-down" : "chevron-up"} size={20} color="#333" />
            </TouchableOpacity>
            <Collapsible collapsed={faq.collapsed}>
              <Text style={{fontSize: 16, color: "#A0A0A0", paddingTop: 4, paddingHorizontal: 5,lineHeight: 18,fontFamily: "Poppins-Regular"}}>{faq.answer}</Text>
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
    { id: "2", title: "Driving Without a \nNumber Plate", amount: "₹ 500",details:"	For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "3", title: "Parking in \nNo-parking Zone", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs.1500" },
    { id: "4", title: "Failure to Intimate Change\nof the Address \nof Vehicle Owner", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "5", title: "Driving Without a \nSeatbelt", amount: "₹ 1000" },
    { id: "6", title: "Triple Riding on \nTwo-vehicle", amount: "₹ 1000" },
    { id: "7", title: "Driving Without Helmet", amount: "₹ 1000" },
    { id: "8", title: "Over-speeding", amount: "₹ 1000",details:"For Light motor vehicle (LMV) Rs. 1000, For Medium passenger goods vehicle Rs. 2000" },
    { id: "9", title: "Violation of Road \nRegulations", amount: "₹ 1000" },
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
        <Text style={{ fontSize: 16, fontWeight: "700", color: "#FFFBFB", marginLeft: 12,fontFamily:"Poppins" }}>
          Traffic Fines
        </Text>
      </View>

    {/* Select your State in Fines */}
    <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 10,
          
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#EEEEEE",
          borderWidth:1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          width: "90%",
          marginLeft:20,
          marginTop:20,
        }}
      >
        <Text style={{ color:"#AFAFAF", fontSize: 14,marginLeft:10,fontFamily:"Poppins" }}>
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
          padding: 9, 
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
      fontWeight: "bold",

      color: "#333",
      fontFamily: "Poppins_600SemiBold",
      
    }}>
      Traffic fines in Maharashtra 
    </Text>
    
    <Text style={{
      fontSize: 8, 
      color: "#666",
      marginTop: 5,
      fontFamily: "Poppins_400Regular",
    }}>
      List of traffic fines as per Motor Vehicle (Amendment) Act
    </Text>
  </View>
</View>


<View style={{ flex: 1, backgroundColor: "#FFFFFF", padding: 10 }}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        contentContainerStyle={{ paddingBottom: 20 }}
        renderItem={({ item }) => {
          const isExpanded = expandedIds.includes(item.id);
          return (
            <View
              style={{
                backgroundColor: "#FFFFFF",
                borderRadius: 15,
                marginBottom: 10,
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
              <TouchableOpacity
                onPress={() => toggleExpand(item.id)}
                
                style={{
                  flexDirection: "row",
                  alignItems: "center",
                  justifyContent: "space-between",
                  position: "relative",
                   
                }}
              >
                <Text
                  style={{
                    fontSize: 14,
                    color: "#393939",
                    flex: 1,
                    fontFamily:"Poppins",
                    textAlign: "left",
                    lineHeight: 20,
                    justifyContent: "center", 
                   
                  }}
                >
                  {item.title}
                </Text>

                 
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

                    <View style={{ flex: 1, alignItems: "center",justifyContent: "center", }}>
                        <Text
                          style={{
                            fontSize: 14,
                            fontWeight: "600",
                            color: "#1A9E75",
                            textAlign: "center",
                            marginLeft:66,
                          }}
                        >
                          {item.amount}
                        </Text>
                      </View>
                <View style={{ flex: 0.2,alignItems: "flex-end"}}>
                {item.details && (
                <Ionicons
                  name={isExpanded ? "chevron-up" : "chevron-down"}
                  size={20} color="#393939"/>
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
                  <Text style={{ fontSize: 15, color: "#A0A0A0",marginLeft:15 }}>
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
          <Text style={{ fontSize: 18, fontWeight: "700", color: "white", marginLeft: 16 }}>Challan Details</Text>
        </View>
  
        
        <View style={{ flexDirection: "row", justifyContent: "space-around", paddingHorizontal:18,marginTop: 10,borderBottomWidth: 1, borderColor: "#ddd",marginLeft:10,marginRight:10 }}>
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Pending" ? "#1A9E75" : "#666" }}>Pending</Text>
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
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Paid" ? "#1A9E75" : "#666" }}>Paid</Text>
          </TouchableOpacity>
          
        </View>
  
      
        {activeTab === "Pending" ? (
          <FlatList
            data={challans}
            keyExtractor={(item) => item.id}
            renderItem={({ item }) => (
              <View style={{backgroundColor: "white", margin: 10, padding: 15, borderRadius: 15, elevation: 2}}>
      
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#000"}}>{item.vehicleNumber}</Text>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#1A9E75"}}>
                {item.amount} 
                </Text>
              </View>

              {/* Due Date */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                
                <Text style={{ fontSize: 14, color: "#888", flex: 1 }}>
                  Due Date: {item.dueDate}
                </Text>
        
                <LinearGradient 
                colors={["#E44E2D", "#E4852E"]} 
                start={{ x: 0, y: 0 }} 
                end={{ x: 1, y: 0 }} 
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
              <View style={{height: 1, backgroundColor: "#ddd", marginVertical: 8 }} />

              {/* Challan ID & Buttons */}
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <View style={{backgroundColor: "#F5F5F5", paddingVertical: 8, paddingHorizontal: 10, borderRadius: 10}}>
                  <Text style={{fontSize: 11, color: "#393939"}}>{item.challanId}</Text>
                </View>

                <View style={{flexDirection: "row", alignItems: "center"}}>
                  <TouchableOpacity style={{flexDirection: "row", alignItems: "center", marginRight: 10}}>
                    <Text style={{color: "#1A9E75", fontSize: 12}}>More Info</Text>
                    <Ionicons name="chevron-down" size={14} color="#1A9E75" />
                  </TouchableOpacity>

                  <TouchableOpacity style={{backgroundColor: "#1A9E75", paddingVertical: 5, paddingHorizontal: 11, borderRadius: 8}}>
                    <Text style={{color: "#F0FFFA", fontFamily:"Medium", fontSize: 10}}>Pay Now</Text>
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









































import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView, Modal, FlatList,Image,StyleSheet,Share,StatusBar   } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { LinearGradient } from 'expo-linear-gradient';
import * as Font from "expo-font";
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
  const [expanded, setExpanded] = useState(null);
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

  // const toggleExpand = (index) => {
  //   setExpanded(expanded === index ? null : index);
  // };

  

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
  <TouchableOpacity onPress={() => navigation.goBack()} style={{ position: "absolute", left: 17 }}>
    <Ionicons name="arrow-back" size={24} color="white" />
  </TouchableOpacity>
  <Text 
    style={{ 
      fontSize: 16, 
      fontWeight: "700", 
      color: "#FFFBFB", 
      fontFamily: 'Poppins',
      marginLeft: 56, 
    }}
  >
    Pay Challan
  </Text>
</View>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 15, marginTop: 17, elevation: 2, marginHorizontal: 20,paddingBottom: 20,paddingTop: 11 }}>
        <Text style={{ fontSize: 16, fontWeight: "medium", marginBottom: 5,fontFamily: 'Poppins' }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8,fontWeight: "Normal",lineHeight:20  }}>Enter your vehicle number and check {"\n"}your challans</Text>
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
        backgroundColor: "#E6F9F0",
        borderWidth: 1.5,
        borderColor: isValid ? "#1A9E75" : "red",
        borderColor: "#1A9E75",
        borderRadius: 8,
        paddingHorizontal: 8,
        color: "#1A9E75",
        fontWeight: "bold",
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
      <Text style={{ color: "white", fontWeight: "bold" }}>Check</Text>
    </TouchableOpacity>
  </View>
   {/* Error Message (Only show if there's an error) */}
   {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5,alignSelf: "flex-start" }}>
            {errorMessage}
          </Text>
        )}
</View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Normal", marginBottom: 8,marginTop: -5 }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        {/* Step 1 */}
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
    {/* Text (Number Plate Content) */}
    <SvgText x="90" y="55" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Enter your {"\n"} vehicle number</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        {/* Step 2 */}
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
    {/* Document Shape with Right-Bottom Cut */}
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />
    {/* Folded Corner */}
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    {/* Text Lines */}
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    {/* Challan Label */}
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Check Your {"\n"} Challans</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        {/* Step 3 */}
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
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 3</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14 }}>Make {"\n"} Payment</Text>
        </View>
      </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, marginTop: 3, elevation: 2,width: "90%",marginHorizontal: 20 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8 }}>Traffic Fines</Text>{/* ,color:"#393939"*/}
        <Text style={{fontSize: 14,
                      color: "#AFAFAF",
                      marginBottom: 8,marginTop: -3}}>Select your state to see traffic fines</Text>
        
      {/* Button to Open Modal */}
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
      fontFamily: "Arial",
      
    }}>
      Invite your friends to make hassle-free
    </Text>
  </View>

      {/* Main Content - Text and Image */}
      <View style={{flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",}}>
        {/* Left Section - Text Content */}
        <View style={{flex: 1,
                      paddingRight: 10,}}>
          <Text style={{color: "white",
    fontSize: 18,
    fontFamily:"Poppins",
    fontWeight: "bold",
    marginBottom: 5,marginLeft:15}}>Challan Payments</Text>
          <Text style={{color: "white",
    fontSize: 10,
    marginBottom: 10,
    marginLeft:15}}>Pay using UPI, Debit & Credit Cards</Text>

          {/* Invite Now Button */}
          <TouchableOpacity style={{backgroundColor: "white",
                                    paddingVertical: 6,
                                    paddingHorizontal: 10,
                                    borderRadius: 15,
                                    alignItems: "center",
                                    alignSelf: "flex-start",marginLeft:15}} 
                                    onPress={shareReferral}>
          <Text style={{alignItems: "flex-end",fontSize:10,fontWeight:"bold",fontFamily:"Poppins"}}>Invite Now</Text>
        </TouchableOpacity>
        </View>

        {/* Right Section - Image */}
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
        <Text style={{fontSize: 18, fontWeight: "bold", color: "#333", marginBottom: 10}}>FAQs</Text>
        {faqs.map((faq, index) => (
          <View key={index} style={{backgroundColor: "#FFFFFF", borderRadius: 10, marginBottom: 10, padding: 10, elevation: 2}}>
            <TouchableOpacity style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center", paddingVertical: 10 }} onPress={() => toggleFAQ(index)}>
              <Text style={{fontSize: 15, fontWeight: "600", color: "#333", fontFamily: "Calibri",lineHeight:20}}>{faq.question}</Text>
              <Ionicons name={faq.collapsed ? "chevron-down" : "chevron-up"} size={20} color="#333" />
            </TouchableOpacity>
            <Collapsible collapsed={faq.collapsed}>
              <Text style={{fontSize: 16, color: "#A0A0A0", paddingTop: 4, paddingHorizontal: 5,lineHeight: 18,fontFamily: "Poppins-Regular"}}>{faq.answer}</Text>
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
    { id: "2", title: "Driving Without a \nNumber Plate", amount: "₹ 500",details:"	For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "3", title: "Parking in \nNo-parking Zone", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs.1500" },
    { id: "4", title: "Failure to Intimate Change\nof the Address \nof Vehicle Owner", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "5", title: "Driving Without a \nSeatbelt", amount: "₹ 1000" },
    { id: "6", title: "Triple Riding on \nTwo-vehicle", amount: "₹ 1000" },
    { id: "7", title: "Driving Without Helmet", amount: "₹ 1000" },
    { id: "8", title: "Over-speeding", amount: "₹ 1000",details:"For Light motor vehicle (LMV) Rs. 1000, For Medium passenger goods vehicle Rs. 2000" },
    { id: "9", title: "Violation of Road \nRegulations", amount: "₹ 1000" },
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
        <Text style={{ fontSize: 16, fontWeight: "700", color: "#FFFBFB", marginLeft: 12,fontFamily:"Poppins" }}>
          Traffic Fines
        </Text>
      </View>

    {/* Select your State in Fines */}
    <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 10,
          
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#EEEEEE",
          borderWidth:1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          width: "90%",
          marginLeft:20,
          marginTop:20,
        }}
      >
        <Text style={{ color:"#AFAFAF", fontSize: 14,marginLeft:10,fontFamily:"Poppins" }}>
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
          padding: 9, 
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
      fontWeight: "bold",

      color: "#333",
      fontFamily: "Poppins_600SemiBold",
      
    }}>
      Traffic fines in Maharashtra 
    </Text>
    
    <Text style={{
      fontSize: 8, 
      color: "#666",
      marginTop: 5,
      fontFamily: "Poppins_400Regular",
    }}>
      List of traffic fines as per Motor Vehicle (Amendment) Act
    </Text>
  </View>
</View>


<View style={{ flex: 1, backgroundColor: "#FFFFFF", padding: 10 }}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        contentContainerStyle={{ paddingBottom: 20 }}
        renderItem={({ item }) => {
          const isExpanded = expandedIds.includes(item.id);
          return (
            <View
              style={{
                backgroundColor: "#FFFFFF",
                borderRadius: 15,
                marginBottom: 10,
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
              <TouchableOpacity
                onPress={() => toggleExpand(item.id)}
                
                style={{
                  flexDirection: "row",
                  alignItems: "center",
                  justifyContent: "space-between",
                  position: "relative",
                   
                }}
              >
                <Text
                  style={{
                    fontSize: 14,
                    color: "#393939",
                    flex: 1,
                    fontFamily:"Poppins",
                    textAlign: "left",
                    lineHeight: 20,
                    justifyContent: "center", 
                   
                  }}
                >
                  {item.title}
                </Text>

                 
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

                    <View style={{ flex: 1, alignItems: "center",justifyContent: "center", }}>
                        <Text
                          style={{
                            fontSize: 14,
                            fontWeight: "600",
                            color: "#1A9E75",
                            textAlign: "center",
                            marginLeft:66,
                          }}
                        >
                          {item.amount}
                        </Text>
                      </View>
                <View style={{ flex: 0.2,alignItems: "flex-end"}}>
                {item.details && (
                <Ionicons
                  name={isExpanded ? "chevron-up" : "chevron-down"}
                  size={20} color="#393939"/>
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
                  <Text style={{ fontSize: 15, color: "#A0A0A0",marginLeft:15 }}>
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
          <Text style={{ fontSize: 18, fontWeight: "700", color: "white", marginLeft: 16 }}>Challan Details</Text>
        </View>
  
        
        <View style={{ flexDirection: "row", justifyContent: "space-around", paddingHorizontal:18,marginTop: 10,borderBottomWidth: 1, borderColor: "#ddd",marginLeft:10,marginRight:10 }}>
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Pending" ? "#1A9E75" : "#666" }}>Pending</Text>
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
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Paid" ? "#1A9E75" : "#666" }}>Paid</Text>
          </TouchableOpacity>
          
        </View>
  
      
        {activeTab === "Pending" ? (
          <FlatList
            data={challans}
            keyExtractor={(item) => item.id}
            renderItem={({ item }) => (
              <View style={{backgroundColor: "white", margin: 10, padding: 15, borderRadius: 15, elevation: 2}}>
      
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#000"}}>{item.vehicleNumber}</Text>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#1A9E75"}}>
                {item.amount} 
                </Text>
              </View>

              {/* Due Date */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                
                <Text style={{ fontSize: 14, color: "#888", flex: 1 }}>
                  Due Date: {item.dueDate}
                </Text>
        
                <LinearGradient 
                colors={["#E44E2D", "#E4852E"]} 
                start={{ x: 0, y: 0 }} 
                end={{ x: 1, y: 0 }} 
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
              <View style={{height: 1, backgroundColor: "#ddd", marginVertical: 8 }} />

              {/* Challan ID & Buttons */}
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <View style={{backgroundColor: "#F5F5F5", paddingVertical: 8, paddingHorizontal: 10, borderRadius: 10}}>
                  <Text style={{fontSize: 11, color: "#393939"}}>{item.challanId}</Text>
                </View>

                <View style={{flexDirection: "row", alignItems: "center"}}>
                  <TouchableOpacity style={{flexDirection: "row", alignItems: "center", marginRight: 10}}>
                    <Text style={{color: "#1A9E75", fontSize: 12}}>More Info</Text>
                    <Ionicons name="chevron-down" size={14} color="#1A9E75" />
                  </TouchableOpacity>

                  <TouchableOpacity style={{backgroundColor: "#1A9E75", paddingVertical: 5, paddingHorizontal: 11, borderRadius: 8}}>
                    <Text style={{color: "#F0FFFA", fontFamily:"Medium", fontSize: 10}}>Pay Now</Text>
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








































































import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView, Modal, FlatList,Image,StyleSheet,Share,StatusBar   } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { LinearGradient } from 'expo-linear-gradient';
import * as Font from "expo-font";
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
  const [expanded, setExpanded] = useState(null);
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

  const toggleExpand = (index) => {
    setExpanded(expanded === index ? null : index);
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
  <TouchableOpacity onPress={() => navigation.goBack()} style={{ position: "absolute", left: 17 }}>
    <Ionicons name="arrow-back" size={24} color="white" />
  </TouchableOpacity>
  <Text 
    style={{ 
      fontSize: 16, 
      fontWeight: "700", 
      color: "#FFFBFB", 
      fontFamily: 'Poppins',
      marginLeft: 56, 
    }}
  >
    Pay Challan
  </Text>
</View>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 15, marginTop: 17, elevation: 2, marginHorizontal: 20,paddingBottom: 20,paddingTop: 11 }}>
        <Text style={{ fontSize: 16, fontWeight: "medium", marginBottom: 5,fontFamily: 'Poppins' }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8,fontWeight: "Normal",lineHeight:20  }}>Enter your vehicle number and check {"\n"}your challans</Text>
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
        backgroundColor: "#E6F9F0",
        borderWidth: 1.5,
        borderColor: isValid ? "#1A9E75" : "red",
        borderColor: "#1A9E75",
        borderRadius: 8,
        paddingHorizontal: 8,
        color: "#1A9E75",
        fontWeight: "bold",
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
      <Text style={{ color: "white", fontWeight: "bold" }}>Check</Text>
    </TouchableOpacity>
  </View>
   {/* Error Message (Only show if there's an error) */}
   {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5,alignSelf: "flex-start" }}>
            {errorMessage}
          </Text>
        )}
</View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Normal", marginBottom: 8,marginTop: -5 }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        {/* Step 1 */}
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
    {/* Text (Number Plate Content) */}
    <SvgText x="90" y="55" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Enter your {"\n"} vehicle number</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        {/* Step 2 */}
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
    {/* Document Shape with Right-Bottom Cut */}
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />
    {/* Folded Corner */}
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    {/* Text Lines */}
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    {/* Challan Label */}
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Check Your {"\n"} Challans</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        {/* Step 3 */}
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
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 3</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14 }}>Make {"\n"} Payment</Text>
        </View>
      </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, marginTop: 3, elevation: 2,width: "90%",marginHorizontal: 20 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8 }}>Traffic Fines</Text>{/* ,color:"#393939"*/}
        <Text style={{fontSize: 14,
                      color: "#AFAFAF",
                      marginBottom: 8,marginTop: -3}}>Select your state to see traffic fines</Text>
        
      {/* Button to Open Modal */}
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
      fontFamily: "Arial",
      
    }}>
      Invite your friends to make hassle-free
    </Text>
  </View>

      {/* Main Content - Text and Image */}
      <View style={{flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",}}>
        {/* Left Section - Text Content */}
        <View style={{flex: 1,
                      paddingRight: 10,}}>
          <Text style={{color: "white",
    fontSize: 18,
    fontFamily:"Poppins",
    fontWeight: "bold",
    marginBottom: 5,marginLeft:15}}>Challan Payments</Text>
          <Text style={{color: "white",
    fontSize: 10,
    marginBottom: 10,
    marginLeft:15}}>Pay using UPI, Debit & Credit Cards</Text>

          {/* Invite Now Button */}
          <TouchableOpacity style={{backgroundColor: "white",
                                    paddingVertical: 6,
                                    paddingHorizontal: 10,
                                    borderRadius: 15,
                                    alignItems: "center",
                                    alignSelf: "flex-start",marginLeft:15}} 
                                    onPress={shareReferral}>
          <Text style={{alignItems: "flex-end",fontSize:10,fontWeight:"bold",fontFamily:"Poppins"}}>Invite Now</Text>
        </TouchableOpacity>
        </View>

        {/* Right Section - Image */}
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
        <Text style={{fontSize: 18, fontWeight: "bold", color: "#333", marginBottom: 10}}>FAQs</Text>
        {faqs.map((faq, index) => (
          <View key={index} style={{backgroundColor: "#FFFFFF", borderRadius: 10, marginBottom: 10, padding: 10, elevation: 2}}>
            <TouchableOpacity style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center", paddingVertical: 10 }} onPress={() => toggleFAQ(index)}>
              <Text style={{fontSize: 15, fontWeight: "600", color: "#333", fontFamily: "Calibri",lineHeight:20}}>{faq.question}</Text>
              <Ionicons name={faq.collapsed ? "chevron-down" : "chevron-up"} size={20} color="#333" />
            </TouchableOpacity>
            <Collapsible collapsed={faq.collapsed}>
              <Text style={{fontSize: 16, color: "#A0A0A0", paddingTop: 4, paddingHorizontal: 5,lineHeight: 18,fontFamily: "Poppins-Regular"}}>{faq.answer}</Text>
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
  const [expandedId, setExpandedId] = useState(null);
  const finesData = [
    { id: "1", title: "Carrying Excess Luggage", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "2", title: "Driving Without a \nNumber Plate", amount: "₹ 500",details:"	For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "3", title: "Parking in \nNo-parking Zone", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs.1500" },
    { id: "4", title: "Failure to Intimate Change\nof the Address \nof Vehicle Owner", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "5", title: "Driving Without a \nSeatbelt", amount: "₹ 1000" },
    { id: "6", title: "Triple Riding on \nTwo-vehicle", amount: "₹ 1000" },
    { id: "7", title: "Driving Without Helmet", amount: "₹ 1000" },
    { id: "8", title: "Over-speeding", amount: "₹ 1000",details:"For Light motor vehicle (LMV) Rs. 1000, For Medium passenger goods vehicle Rs. 2000" },
    { id: "9", title: "Violation of Road \nRegulations", amount: "₹ 1000" },
  ];

  return (
    <View style={{ flex: 1, backgroundColor: "#FFFFFF" }}>
      {/* Header */}
      <View style={{ backgroundColor: "#1A9E75", height: 88, flexDirection: "row", alignItems: "center", paddingHorizontal: 13, paddingTop: 25 }}>
        <TouchableOpacity onPress={() => navigation.goBack()}>
          <Ionicons name="arrow-back" size={24} color="white" />
        </TouchableOpacity>
        <Text style={{ fontSize: 16, fontWeight: "700", color: "#FFFBFB", marginLeft: 12,fontFamily:"Poppins" }}>
          Traffic Fines
        </Text>
      </View>

    {/* Select your State in Fines */}
    <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 10,
          
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#EEEEEE",
          borderWidth:1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          width: "90%",
          marginLeft:20,
          marginTop:20,
        }}
      >
        <Text style={{ color:"#AFAFAF", fontSize: 14,marginLeft:10,fontFamily:"Poppins" }}>
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
          padding: 9, 
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
      fontWeight: "bold",

      color: "#333",
      fontFamily: "Poppins_600SemiBold",
      
    }}>
      Traffic fines in Maharashtra 
    </Text>
    
    <Text style={{
      fontSize: 8, 
      color: "#666",
      marginTop: 5,
      fontFamily: "Poppins_400Regular",
    }}>
      List of traffic fines as per Motor Vehicle (Amendment) Act
    </Text>
  </View>
</View>


<View style={{ flex: 1, backgroundColor: "#FFFFFF", padding: 10 }}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        contentContainerStyle={{ paddingBottom: 20 }}
        renderItem={({ item }) => {
          const isExpanded = expandedId === item.id;
          return (
            <View
              style={{
                backgroundColor: "#FFFFFF",
                borderRadius: 15,
                marginBottom: 10,
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
              <TouchableOpacity
                onPress={() =>
                  setExpandedId(isExpanded ? null : item.id)
                }
                style={{
                  flexDirection: "row",
                  alignItems: "center",
                  justifyContent: "space-between",
                  position: "relative",
                   
                }}
              >
                <Text
                  style={{
                    fontSize: 14,
                    color: "#393939",
                    flex: 1,
                    fontFamily:"Poppins",
                    textAlign: "left",
                    lineHeight: 20,
                    justifyContent: "center", 
                   
                  }}
                >
                  {item.title}
                </Text>

                 
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

                    <View style={{ flex: 1, alignItems: "center",justifyContent: "center", }}>
                        <Text
                          style={{
                            fontSize: 14,
                            fontWeight: "600",
                            color: "#1A9E75",
                            textAlign: "center",
                            marginLeft:66,
                          }}
                        >
                          {item.amount}
                        </Text>
                      </View>
                <View style={{ flex: 0.2,
                                alignItems: "flex-end"}}>
                {item.details && (
                <Ionicons
                  name={isExpanded ? "chevron-up" : "chevron-down"}
                  size={20}
                  color="#393939"

                />
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
                  <Text style={{ fontSize: 15, color: "#A0A0A0",marginLeft:15 }}>
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
          <Text style={{ fontSize: 18, fontWeight: "700", color: "white", marginLeft: 16 }}>Challan Details</Text>
        </View>
  
        
        <View style={{ flexDirection: "row", justifyContent: "space-around", paddingHorizontal:18,marginTop: 10,borderBottomWidth: 1, borderColor: "#ddd",marginLeft:10,marginRight:10 }}>
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Pending" ? "#1A9E75" : "#666" }}>Pending</Text>
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
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Paid" ? "#1A9E75" : "#666" }}>Paid</Text>
          </TouchableOpacity>
          
        </View>
  
      
        {activeTab === "Pending" ? (
          <FlatList
            data={challans}
            keyExtractor={(item) => item.id}
            renderItem={({ item }) => (
              <View style={{backgroundColor: "white", margin: 10, padding: 15, borderRadius: 15, elevation: 2}}>
      
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#000"}}>{item.vehicleNumber}</Text>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#1A9E75"}}>
                {item.amount} 
                </Text>
              </View>

              {/* Due Date */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                
                <Text style={{ fontSize: 14, color: "#888", flex: 1 }}>
                  Due Date: {item.dueDate}
                </Text>
        
                <LinearGradient 
                colors={["#E44E2D", "#E4852E"]} 
                start={{ x: 0, y: 0 }} 
                end={{ x: 1, y: 0 }} 
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
              <View style={{height: 1, backgroundColor: "#ddd", marginVertical: 8 }} />

              {/* Challan ID & Buttons */}
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <View style={{backgroundColor: "#F5F5F5", paddingVertical: 8, paddingHorizontal: 10, borderRadius: 10}}>
                  <Text style={{fontSize: 11, color: "#393939"}}>{item.challanId}</Text>
                </View>

                <View style={{flexDirection: "row", alignItems: "center"}}>
                  <TouchableOpacity style={{flexDirection: "row", alignItems: "center", marginRight: 10}}>
                    <Text style={{color: "#1A9E75", fontSize: 12}}>More Info</Text>
                    <Ionicons name="chevron-down" size={14} color="#1A9E75" />
                  </TouchableOpacity>

                  <TouchableOpacity style={{backgroundColor: "#1A9E75", paddingVertical: 5, paddingHorizontal: 11, borderRadius: 8}}>
                    <Text style={{color: "#F0FFFA", fontFamily:"Medium", fontSize: 10}}>Pay Now</Text>
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















































































import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView, Modal, FlatList,Image,StyleSheet,Share,StatusBar   } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { LinearGradient } from 'expo-linear-gradient';
import * as Font from "expo-font";
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
  const [expanded, setExpanded] = useState(null);
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

  const toggleExpand = (index) => {
    setExpanded(expanded === index ? null : index);
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
      setErrorMessage(""); // ✅ Remove error when valid
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
  <TouchableOpacity onPress={() => navigation.goBack()} style={{ position: "absolute", left: 17 }}>
    <Ionicons name="arrow-back" size={24} color="white" />
  </TouchableOpacity>
  <Text 
    style={{ 
      fontSize: 16, 
      fontWeight: "700", 
      color: "#FFFBFB", 
      fontFamily: 'Poppins',
      marginLeft: 56, // Increased left margin to push text right
    }}
  >
    Pay Challan
  </Text>
</View>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 15, marginTop: 17, elevation: 2, marginHorizontal: 20,paddingBottom: 20,paddingTop: 11 }}>
        <Text style={{ fontSize: 16, fontWeight: "medium", marginBottom: 5,fontFamily: 'Poppins' }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8,fontWeight: "Normal",lineHeight:20  }}>Enter your vehicle number and check {"\n"}your challans</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5, borderRadius: 8 }}>
    <TextInput
      placeholder="Enter Your Vehicle Number"
      value={vehicleNumber}
      onChangeText={validateInput}
      maxLength={10}
      placeholderTextColor="#1A9E75"
      style={{
        width: 190, // Set fixed width instead of flex
        height: 40,
        backgroundColor: "#E6F9F0",
        borderWidth: 1.5,
        borderColor: isValid ? "#1A9E75" : "red",
        borderColor: "#1A9E75",
        borderRadius: 8,
        paddingHorizontal: 8,
        color: "#1A9E75",
        fontWeight: "bold",
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
      <Text style={{ color: "white", fontWeight: "bold" }}>Check</Text>
    </TouchableOpacity>
  </View>
   {/* Error Message (Only show if there's an error) */}
   {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5,alignSelf: "flex-start" }}>
            {errorMessage}
          </Text>
        )}
</View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Normal", marginBottom: 8,marginTop: -5 }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        {/* Step 1 */}
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
    {/* Outer Rounded Rectangle (Main Plate) */}
    <Rect x="10" y="20" width="160" height="60" rx="10" fill="#1A9E75" />

    {/* Inner White Rectangle (Text Background) */}
    <Rect x="20" y="30" width="140" height="40" rx="6" fill="white" />

    {/* Small Circles (Decorative Bolts) */}
    <Circle cx="28" cy="36" r="4" fill="#1A9E75" />
    <Circle cx="152" cy="36" r="4" fill="#1A9E75" />
    <Circle cx="28" cy="62" r="4" fill="#1A9E75" />
    <Circle cx="152" cy="62" r="4" fill="#1A9E75" />

    {/* Text (Number Plate Content) */}
    <SvgText x="90" y="55" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
    








            {/* <MaterialCommunityIcons name="license-plate" size={40} color="#1A9E75" /> */}
            {/* <Ionicons name="car-outline" size={30} color="#1A9E75" /> */}
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Enter your {"\n"} vehicle number</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        {/* Step 2 */}
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
    {/* Document Shape with Right-Bottom Cut */}
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />

    {/* Folded Corner */}
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    {/* Text Lines */}
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    {/* Challan Label */}
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
            {/* <Ionicons name="document-text-outline" size={30} color="#1A9E75" /> */}
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Check Your {"\n"} Challans</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        {/* Step 3 */}
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
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 3</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14 }}>Make {"\n"} Payment</Text>
        </View>
      </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, marginTop: 3, elevation: 2,width: "90%",marginHorizontal: 20 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8 }}>Traffic Fines</Text>{/* ,color:"#393939"*/}
        <Text style={{fontSize: 14,
                      color: "#AFAFAF",
                      marginBottom: 8,marginTop: -3}}>Select your state to see traffic fines</Text>
        
      

      {/* drop down state */}
      

  
      
      {/* Button to Open Modal */}
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
                        marginTop: 25, // 🔥 Decreased top space
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
      fontFamily: "Arial",
      
    }}>
      Invite your friends to make hassle-free
    </Text>
  </View>

      {/* Main Content - Text and Image */}
      <View style={{flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",}}>
        {/* Left Section - Text Content */}
        <View style={{flex: 1,
                      paddingRight: 10,}}>
          <Text style={{color: "white",
    fontSize: 18,
    fontFamily:"Poppins",
    fontWeight: "bold",
    marginBottom: 5,marginLeft:15}}>Challan Payments</Text>
          <Text style={{color: "white",
    fontSize: 10,
    marginBottom: 10,
    marginLeft:15}}>Pay using UPI, Debit & Credit Cards</Text>

          {/* Invite Now Button */}
          <TouchableOpacity style={{backgroundColor: "white",
                                    paddingVertical: 6,
                                    paddingHorizontal: 10,
                                    borderRadius: 15,
                                    alignItems: "center",
                                    alignSelf: "flex-start",marginLeft:15}} 
                                    onPress={shareReferral}>
          <Text style={{alignItems: "flex-end",fontSize:10,fontWeight:"bold",fontFamily:"Poppins"}}>Invite Now</Text>
        </TouchableOpacity>
        </View>

        {/* Right Section - Image */}
        <View style={{width: 80,
                      height: 100,
                      alignItems: "center",
                      justifyContent: "center",
                      borderBottomRightRadius: 1, 
                      paddingTop: 28, 
                      
                       }}>
          <Image
            source={require("../../assets/images/mega1.jpg")}  //mega1.jpg
            style={{width: 120,
              height: 90,
              resizeMode: "contain",}}
          />
        </View>
      </View>
      
    </LinearGradient>
       
       <View style={{marginTop: 15,marginHorizontal:20 }}>
        <Text style={{fontSize: 18, fontWeight: "bold", color: "#333", marginBottom: 10}}>FAQs</Text>
        {faqs.map((faq, index) => (
          <View key={index} style={{backgroundColor: "#FFFFFF", borderRadius: 10, marginBottom: 10, padding: 10, elevation: 2}}>
            <TouchableOpacity style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center", paddingVertical: 10 }} onPress={() => toggleFAQ(index)}>
              <Text style={{fontSize: 15, fontWeight: "600", color: "#333", fontFamily: "Calibri",lineHeight:20}}>{faq.question}</Text>
              <Ionicons name={faq.collapsed ? "chevron-down" : "chevron-up"} size={20} color="#333" />
            </TouchableOpacity>
            <Collapsible collapsed={faq.collapsed}>
              <Text style={{fontSize: 16, color: "#A0A0A0", paddingTop: 4, paddingHorizontal: 5,lineHeight: 18,fontFamily: "Poppins-Regular"}}>{faq.answer}</Text>
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
  const [expandedId, setExpandedId] = useState(null);
  const finesData = [
    { id: "1", title: "Carrying Excess Luggage", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "2", title: "Driving Without a \nNumber Plate", amount: "₹ 500",details:"	For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "3", title: "Parking in \nNo-parking Zone", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs.1500" },
    { id: "4", title: "Failure to Intimate Change\nof the Address \nof Vehicle Owner", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "5", title: "Driving Without a \nSeatbelt", amount: "₹ 1000" },
    { id: "6", title: "Triple Riding on \nTwo-vehicle", amount: "₹ 1000" },
    { id: "7", title: "Driving Without Helmet", amount: "₹ 1000" },
    { id: "8", title: "Over-speeding", amount: "₹ 1000",details:"For Light motor vehicle (LMV) Rs. 1000, For Medium passenger goods vehicle Rs. 2000" },
    { id: "9", title: "Violation of Road \nRegulations", amount: "₹ 1000" },
  ];

  return (
    <View style={{ flex: 1, backgroundColor: "#FFFFFF" }}>
      {/* Header */}
      <View style={{ backgroundColor: "#1A9E75", height: 88, flexDirection: "row", alignItems: "center", paddingHorizontal: 13, paddingTop: 25 }}>
        <TouchableOpacity onPress={() => navigation.goBack()}>
          <Ionicons name="arrow-back" size={24} color="white" />
        </TouchableOpacity>
        <Text style={{ fontSize: 16, fontWeight: "700", color: "#FFFBFB", marginLeft: 12,fontFamily:"Poppins" }}>
          Traffic Fines
        </Text>
      </View>

    {/* Select your State in Fines */}


    <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 10,
          
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#EEEEEE",
          borderWidth:1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          width: "91%",
          marginLeft:20,
          marginTop:20,
        }}
      >
        <Text style={{ color:"#AFAFAF", fontSize: 14,marginLeft:10,fontFamily:"Poppins" }}>
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
          backgroundColor: "#F0FFFA", // Light greenish background
          padding: 9, // Increased padding for better spacing
          borderRadius: 12,
          width: "91%", // Increased width to make it bigger
          alignSelf: "center", // Centers the container
          marginTop: 10, 
        }}>
  <MaterialCommunityIcons name="traffic-light" size={44} color="#1A9E75" paddingHorizontal="5" />
  
  {/* Text Container */}
  <View style={{ flex: 1, marginLeft: 1 }}>  
    <Text style={{
      fontSize: 16, // Slightly larger font
      fontWeight: "bold",

      color: "#333",
      fontFamily: "Poppins_600SemiBold",
      
    }}>
      Traffic fines in Maharashtra 
    </Text>
    
    <Text style={{
      fontSize: 8, // Slightly larger for better readability
      color: "#666",
      marginTop: 5,
      fontFamily: "Poppins_400Regular",
    }}>
      List of traffic fines as per Motor Vehicle (Amendment) Act
    </Text>
  </View>
</View>


<View style={{ flex: 1, backgroundColor: "#FFFFFF", padding: 10 }}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        contentContainerStyle={{ paddingBottom: 20 }}
        renderItem={({ item }) => {
          const isExpanded = expandedId === item.id;
          return (
            <View
              style={{
                backgroundColor: "#FFFFFF",
                borderRadius: 15,
                marginBottom: 10,
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
              <TouchableOpacity
                onPress={() =>
                  setExpandedId(isExpanded ? null : item.id)
                }
                style={{
                  flexDirection: "row",
                  alignItems: "center",
                  justifyContent: "space-between",
                  position: "relative",
                   
                }}
              >
                <Text
                  style={{
                    fontSize: 14,
                    color: "#393939",
                    flex: 1,
                    fontFamily:"Poppins",
                    textAlign: "left",
                    lineHeight: 20,
                    justifyContent: "center", 
                   
                  }}
                >
                  {item.title}
                </Text>

                 {/* Vertical Separator */}
                 <View
                      style={{
                        height: 27,
                        width: 1,
                        backgroundColor: "#E4E4E4",
                        position: "absolute",
                        left: 175, // Adjust this value as needed for proper alignment
                        top: "50%",
                        transform: [{ translateY: -13.5 }], // Centers vertically
                      }}
                    />

                    <View style={{ flex: 1, alignItems: "center",justifyContent: "center", }}>
                        <Text
                          style={{
                            fontSize: 14,
                            fontWeight: "600",
                            color: "#1A9E75",
                            textAlign: "center",
                            marginLeft:66,
                          }}
                        >
                          {item.amount}
                        </Text>
                      </View>
                <View style={{ flex: 0.2,
                                alignItems: "flex-end"}}>
                {item.details && (
                <Ionicons
                  name={isExpanded ? "chevron-up" : "chevron-down"}
                  size={20}
                  color="#393939"

                />
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
                  <Text style={{ fontSize: 15, color: "#A0A0A0",marginLeft:15 }}>
                    {item.details}
                  </Text>
                </View>
              )}
              </View>
              )}}
              />
              </View>
              </View>
  )
}


  const ChallanDetailsScreen = ({ navigation }) => {
    const [activeTab, setActiveTab] = useState("Pending");
  
    return (
      <View style={{ flex: 1, backgroundColor: "#fff" }}>
        {/* Header */}
        <View style={{ backgroundColor: "#1A9E75", height: 80, flexDirection: "row", alignItems: "center", paddingHorizontal: 16, paddingTop: 30 }}>
          <TouchableOpacity onPress={() => navigation.goBack()}>
            <Ionicons name="arrow-back" size={24} color="white" />
          </TouchableOpacity>
          <Text style={{ fontSize: 18, fontWeight: "700", color: "white", marginLeft: 16 }}>Challan Details</Text>
        </View>
  
        {/* Tabs */}
        <View style={{ flexDirection: "row", justifyContent: "space-around", paddingHorizontal:18,marginTop: 10,borderBottomWidth: 1, borderColor: "#ddd",marginLeft:10,marginRight:10 }}>
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Pending" ? "#1A9E75" : "#666" }}>Pending</Text>
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
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Paid" ? "#1A9E75" : "#666" }}>Paid</Text>
          </TouchableOpacity>
          
        </View>
  
        {/* Challan List */}
        {activeTab === "Pending" ? (
          <FlatList
            data={challans}
            keyExtractor={(item) => item.id}
            renderItem={({ item }) => (
              <View style={{backgroundColor: "white", margin: 10, padding: 15, borderRadius: 15, elevation: 2}}>
      
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#000"}}>{item.vehicleNumber}</Text>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#1A9E75"}}>
                {item.amount} 
                </Text>
              </View>

              {/* Due Date */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                {/* Due Date */}
                <Text style={{ fontSize: 14, color: "#888", flex: 1 }}>
                  Due Date: {item.dueDate}
                </Text>
        
                <LinearGradient 
                colors={["#E44E2D", "#E4852E"]} // Red-Orange Gradient
                start={{ x: 0, y: 0 }} 
                end={{ x: 1, y: 0 }} 
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
              <View style={{height: 1, backgroundColor: "#ddd", marginVertical: 8 }} />

              {/* Challan ID & Buttons */}
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <View style={{backgroundColor: "#F5F5F5", paddingVertical: 8, paddingHorizontal: 10, borderRadius: 10}}>
                  <Text style={{fontSize: 11, color: "#393939"}}>{item.challanId}</Text>
                </View>

                <View style={{flexDirection: "row", alignItems: "center"}}>
                  <TouchableOpacity style={{flexDirection: "row", alignItems: "center", marginRight: 10}}>
                    <Text style={{color: "#1A9E75", fontSize: 12}}>More Info</Text>
                    <Ionicons name="chevron-down" size={14} color="#1A9E75" />
                  </TouchableOpacity>

                  <TouchableOpacity style={{backgroundColor: "#1A9E75", paddingVertical: 5, paddingHorizontal: 11, borderRadius: 8}}>
                    <Text style={{color: "#F0FFFA", fontFamily:"Medium", fontSize: 10}}>Pay Now</Text>
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
    width: "90%", // Responsive width
    marginTop: 15,
    alignSelf: "center",
    marginBottom: 5,
    elevation: 5, // Android shadow
    shadowColor: "#000", // iOS shadow
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.2,
    shadowRadius: 4,
    backgroundColor: "#0BC189",
  },
  fullWidthTextContainer: {
    width: "100%", // Ensures full width
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
    paddingTop: 40, // Adjust top padding if needed
  },
  image: {
    width: 70,
    height: 90,
    resizeMode: "contain",
  },

});





































import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView, Modal, FlatList,Image,StyleSheet,Share,StatusBar   } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { LinearGradient } from 'expo-linear-gradient';
import * as Font from "expo-font";
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
  const [expanded, setExpanded] = useState(null);
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

  const toggleExpand = (index) => {
    setExpanded(expanded === index ? null : index);
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
      setErrorMessage(""); // ✅ Remove error when valid
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
  <TouchableOpacity onPress={() => navigation.goBack()} style={{ position: "absolute", left: 17 }}>
    <Ionicons name="arrow-back" size={24} color="white" />
  </TouchableOpacity>
  <Text 
    style={{ 
      fontSize: 16, 
      fontWeight: "700", 
      color: "#FFFBFB", 
      fontFamily: 'Poppins',
      marginLeft: 56, // Increased left margin to push text right
    }}
  >
    Pay Challan
  </Text>
</View>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 15, marginTop: 17, elevation: 2, marginHorizontal: 20,paddingBottom: 20,paddingTop: 11 }}>
        <Text style={{ fontSize: 16, fontWeight: "medium", marginBottom: 5,fontFamily: 'Poppins' }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8,fontWeight: "Normal",lineHeight:20  }}>Enter your vehicle number and check {"\n"}your challans</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5, borderRadius: 8 }}>
    <TextInput
      placeholder="Enter Your Vehicle Number"
      value={vehicleNumber}
      onChangeText={validateInput}
      maxLength={10}
      placeholderTextColor="#1A9E75"
      style={{
        width: 190, // Set fixed width instead of flex
        height: 40,
        backgroundColor: "#E6F9F0",
        borderWidth: 1.5,
        borderColor: isValid ? "#1A9E75" : "red",
        borderColor: "#1A9E75",
        borderRadius: 8,
        paddingHorizontal: 8,
        color: "#1A9E75",
        fontWeight: "bold",
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
      <Text style={{ color: "white", fontWeight: "bold" }}>Check</Text>
    </TouchableOpacity>
  </View>
   {/* Error Message (Only show if there's an error) */}
   {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5,alignSelf: "flex-start" }}>
            {errorMessage}
          </Text>
        )}
</View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Normal", marginBottom: 8,marginTop: -5 }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        {/* Step 1 */}
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
    {/* Outer Rounded Rectangle (Main Plate) */}
    <Rect x="10" y="20" width="160" height="60" rx="10" fill="#1A9E75" />

    {/* Inner White Rectangle (Text Background) */}
    <Rect x="20" y="30" width="140" height="40" rx="6" fill="white" />

    {/* Small Circles (Decorative Bolts) */}
    <Circle cx="28" cy="36" r="4" fill="#1A9E75" />
    <Circle cx="152" cy="36" r="4" fill="#1A9E75" />
    <Circle cx="28" cy="62" r="4" fill="#1A9E75" />
    <Circle cx="152" cy="62" r="4" fill="#1A9E75" />

    {/* Text (Number Plate Content) */}
    <SvgText x="90" y="55" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
    








            {/* <MaterialCommunityIcons name="license-plate" size={40} color="#1A9E75" /> */}
            {/* <Ionicons name="car-outline" size={30} color="#1A9E75" /> */}
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Enter your {"\n"} vehicle number</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        {/* Step 2 */}
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
    {/* Document Shape with Right-Bottom Cut */}
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />

    {/* Folded Corner */}
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    {/* Text Lines */}
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    {/* Challan Label */}
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
            {/* <Ionicons name="document-text-outline" size={30} color="#1A9E75" /> */}
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Check Your {"\n"} Challans</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        {/* Step 3 */}
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
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 3</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14 }}>Make {"\n"} Payment</Text>
        </View>
      </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, marginTop: 3, elevation: 2,width: "90%",marginHorizontal: 20 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8 }}>Traffic Fines</Text>{/* ,color:"#393939"*/}
        <Text style={{fontSize: 14,
                      color: "#AFAFAF",
                      marginBottom: 8,marginTop: -3}}>Select your state to see traffic fines</Text>
        
      

      {/* drop down state */}
      

  
      
      {/* Button to Open Modal */}
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
                        marginTop: 25, // 🔥 Decreased top space
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
      fontFamily: "Arial",
      
    }}>
      Invite your friends to make hassle-free
    </Text>
  </View>

      {/* Main Content - Text and Image */}
      <View style={{flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",}}>
        {/* Left Section - Text Content */}
        <View style={{flex: 1,
                      paddingRight: 10,}}>
          <Text style={{color: "white",
    fontSize: 18,
    fontFamily:"Poppins",
    fontWeight: "bold",
    marginBottom: 5,marginLeft:15}}>Challan Payments</Text>
          <Text style={{color: "white",
    fontSize: 10,
    marginBottom: 10,
    marginLeft:15}}>Pay using UPI, Debit & Credit Cards</Text>

          {/* Invite Now Button */}
          <TouchableOpacity style={{backgroundColor: "white",
                                    paddingVertical: 6,
                                    paddingHorizontal: 10,
                                    borderRadius: 15,
                                    alignItems: "center",
                                    alignSelf: "flex-start",marginLeft:15}} 
                                    onPress={shareReferral}>
          <Text style={{alignItems: "flex-end",fontSize:10,fontWeight:"bold",fontFamily:"Poppins"}}>Invite Now</Text>
        </TouchableOpacity>
        </View>

        {/* Right Section - Image */}
        <View style={{width: 80,
                      height: 100,
                      alignItems: "center",
                      justifyContent: "center",
                      borderBottomRightRadius: 1, 
                      paddingTop: 28, 
                      
                       }}>
          <Image
            source={require("../../assets/images/mega1.jpg")}  //mega1.jpg
            style={{width: 120,
              height: 90,
              resizeMode: "contain",}}
          />
        </View>
      </View>
      
    </LinearGradient>
       
       <View style={{marginTop: 15,marginHorizontal:20 }}>
        <Text style={{fontSize: 18, fontWeight: "bold", color: "#333", marginBottom: 10}}>FAQs</Text>
        {faqs.map((faq, index) => (
          <View key={index} style={{backgroundColor: "#FFFFFF", borderRadius: 10, marginBottom: 10, padding: 10, elevation: 2}}>
            <TouchableOpacity style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center", paddingVertical: 10 }} onPress={() => toggleFAQ(index)}>
              <Text style={{fontSize: 15, fontWeight: "600", color: "#333", fontFamily: "Calibri",lineHeight:20}}>{faq.question}</Text>
              <Ionicons name={faq.collapsed ? "chevron-down" : "chevron-up"} size={20} color="#333" />
            </TouchableOpacity>
            <Collapsible collapsed={faq.collapsed}>
              <Text style={{fontSize: 16, color: "#A0A0A0", paddingTop: 4, paddingHorizontal: 5,lineHeight: 18,fontFamily: "Poppins-Regular"}}>{faq.answer}</Text>
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
  const [expandedId, setExpandedId] = useState(null);
  const finesData = [
    { id: "1", title: "Carrying Excess Luggage", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "2", title: "Driving Without a \nNumber Plate", amount: "₹ 500",details:"	For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "3", title: "Parking in \nNo-parking Zone", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs.1500" },
    { id: "4", title: "Failure to Intimate Change\nof the Address \nof Vehicle Owner", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "5", title: "Driving Without a \nSeatbelt", amount: "₹ 1000" },
    { id: "6", title: "Triple Riding on \nTwo-vehicle", amount: "₹ 1000" },
    { id: "7", title: "Driving Without Helmet", amount: "₹ 1000" },
    { id: "8", title: "Over-speeding", amount: "₹ 1000",details:"For Light motor vehicle (LMV) Rs. 1000, For Medium passenger goods vehicle Rs. 2000" },
    { id: "9", title: "Violation of Road \nRegulations", amount: "₹ 1000" },
    { id: "10", title: "Driving When Mentally \nor Physically Unfit to Drive", amount: "₹ 1000",details:"For first offence Rs. 1000, For repeat offence Rs. 2000" },
    { id: "11", title: "Driving Without \nInsurance", amount: "₹ 2000" },
    { id: "12", title: "Overloading", amount: "₹ 2000" },
    { id: "13", title: "Driving Uninsured Vehicle", amount: "₹ 2000" },
    { id: "14", title: "Driving Vehicle \nWithout Registration", amount: "₹ 2000" },
    { id: "15", title: "Dangerous/Rash Driving", amount: "₹ 5000",details:"For first offence Rs. 5000, For repeat offence Rs. 10000" },
    { id: "16", title: "Disobey of Traffic \nSignals", amount: "₹ 5000",details:"For first offence Rs. 5000, For repeat offence Rs. 10000" },
    { id: "17", title: "Using a Mobile \nPhone While Driving", amount: "₹ 5000",details:"For the first offence Rs. 5000, For repeat offence Rs. 10000" },
    { id: "18", title: "Racing", amount: "₹ 5000",details:"For the first offence Rs. 5000, For repeat offence Rs. 10000" },
    { id: "19", title: "Driving Without a \nValid Driving Licence", amount: "₹ 5000" },
    { id: "20", title: "Drunk Driving", amount: "₹ 10,000" },
    { id: "21", title: "Carrying Explosive/\nInflammable Substances", amount: "₹ 10,000" },
    { id: "22", title: "Not Giving Passage \nto Emergency Vehicles", amount: "₹ 10,000" },
    { id: "23", title: "Disqualified Person \nDriving a Vehicle", amount: "₹ 10,000" },
     { id: "24", title: "Minor Driving \nVehicle", amount: "₹ 25,000" },
    // { id: "25", title: "Driving a Vehicle \nRegistered in Another \nState for More Than 12 Months", amount: "₹ 500" },
  ];

  return (
    <View style={{ flex: 1, backgroundColor: "#FFFFFF" }}>
      {/* Header */}
      <View style={{ backgroundColor: "#1A9E75", height: 88, flexDirection: "row", alignItems: "center", paddingHorizontal: 13, paddingTop: 25 }}>
        <TouchableOpacity onPress={() => navigation.goBack()}>
          <Ionicons name="arrow-back" size={24} color="white" />
        </TouchableOpacity>
        <Text style={{ fontSize: 16, fontWeight: "700", color: "#FFFBFB", marginLeft: 12,fontFamily:"Poppins" }}>
          Traffic Fines
        </Text>
      </View>

    {/* Select your State in Fines */}


    <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 10,
          
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#EEEEEE",
          borderWidth:1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          width: "91%",
          marginLeft:20,
          marginTop:20,
        }}
      >
        <Text style={{ color:"#AFAFAF", fontSize: 14,marginLeft:10,fontFamily:"Poppins" }}>
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
          backgroundColor: "#F0FFFA", // Light greenish background
          padding: 9, // Increased padding for better spacing
          borderRadius: 12,
          width: "91%", // Increased width to make it bigger
          alignSelf: "center", // Centers the container
          marginTop: 10, 
        }}>
  <MaterialCommunityIcons name="traffic-light" size={44} color="#1A9E75" paddingHorizontal="5" />
  
  {/* Text Container */}
  <View style={{ flex: 1, marginLeft: 1 }}>  
    <Text style={{
      fontSize: 16, // Slightly larger font
      fontWeight: "bold",

      color: "#333",
      fontFamily: "Poppins_600SemiBold",
      
    }}>
      Traffic fines in Maharashtra 
    </Text>
    
    <Text style={{
      fontSize: 8, // Slightly larger for better readability
      color: "#666",
      marginTop: 5,
      fontFamily: "Poppins_400Regular",
    }}>
      List of traffic fines as per Motor Vehicle (Amendment) Act
    </Text>
  </View>
</View>


<View style={{ flex: 1, backgroundColor: "#FFFFFF", padding: 10 }}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        contentContainerStyle={{ paddingBottom: 20 }}
        renderItem={({ item }) => {
          const isExpanded = expandedId === item.id;
          return (
            <View
              style={{
                backgroundColor: "#FFFFFF",
                borderRadius: 15,
                marginBottom: 10,
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
              <TouchableOpacity
                onPress={() =>
                  setExpandedId(isExpanded ? null : item.id)
                }
                style={{
                  flexDirection: "row",
                  alignItems: "center",
                  justifyContent: "space-between",
                  position: "relative",
                  
                }}
              >
                <Text
                  style={{
                    fontSize: 14,
                    color: "#393939",
                    flex: 1,
                    fontFamily:"Poppins",
                    textAlign: "left",
                    lineHeight: 20,
                    justifyContent: "center", 
                    
                  }}
                >
                  {item.title}
                </Text>

                 {/* Vertical Separator */}
                 <View
                      style={{
                        height: 27,
                        width: 1,
                        backgroundColor: "#E4E4E4",
                        position: "absolute",
                        left: 175, // Adjust this value as needed for proper alignment
                        top: "50%",
                        transform: [{ translateY: -13.5 }], // Centers vertically
                      }}
                    />

                    <View style={{ flex: 1, alignItems: "center",justifyContent: "center", }}>
                        <Text
                          style={{
                            fontSize: 14,
                            fontWeight: "600",
                            color: "#1A9E75",
                            textAlign: "center",
                            marginLeft:66,
                          }}
                        >
                          {item.amount}
                        </Text>
                      </View>
                <View style={{ flex: 0.2,
                                alignItems: "flex-end"}}>
                {item.details && (
                <Ionicons
                  name={isExpanded ? "chevron-up" : "chevron-down"}
                  size={20}
                  color="#393939"

                />
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
                  <Text style={{ fontSize: 15, color: "#A0A0A0",marginLeft:15 }}>
                    {item.details}
                  </Text>
                </View>
              )}
              </View>
              )}}
              />
              </View>
              </View>
  )
}


  const ChallanDetailsScreen = ({ navigation }) => {
    const [activeTab, setActiveTab] = useState("Pending");
  
    return (
      <View style={{ flex: 1, backgroundColor: "#fff" }}>
        {/* Header */}
        <View style={{ backgroundColor: "#1A9E75", height: 80, flexDirection: "row", alignItems: "center", paddingHorizontal: 16, paddingTop: 30 }}>
          <TouchableOpacity onPress={() => navigation.goBack()}>
            <Ionicons name="arrow-back" size={24} color="white" />
          </TouchableOpacity>
          <Text style={{ fontSize: 18, fontWeight: "700", color: "white", marginLeft: 16 }}>Challan Details</Text>
        </View>
  
        {/* Tabs */}
        <View style={{ flexDirection: "row", justifyContent: "space-around", paddingHorizontal:18,marginTop: 10,borderBottomWidth: 1, borderColor: "#ddd",marginLeft:10,marginRight:10 }}>
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Pending" ? "#1A9E75" : "#666" }}>Pending</Text>
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
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Paid" ? "#1A9E75" : "#666" }}>Paid</Text>
          </TouchableOpacity>
          
        </View>
  
        {/* Challan List */}
        {activeTab === "Pending" ? (
          <FlatList
            data={challans}
            keyExtractor={(item) => item.id}
            renderItem={({ item }) => (
              <View style={{backgroundColor: "white", margin: 10, padding: 15, borderRadius: 15, elevation: 2}}>
      
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#000"}}>{item.vehicleNumber}</Text>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#1A9E75"}}>
                {item.amount} 
                </Text>
              </View>

              {/* Due Date */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                {/* Due Date */}
                <Text style={{ fontSize: 14, color: "#888", flex: 1 }}>
                  Due Date: {item.dueDate}
                </Text>
        
                <LinearGradient 
                colors={["#E44E2D", "#E4852E"]} // Red-Orange Gradient
                start={{ x: 0, y: 0 }} 
                end={{ x: 1, y: 0 }} 
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
              <View style={{height: 1, backgroundColor: "#ddd", marginVertical: 8 }} />

              {/* Challan ID & Buttons */}
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <View style={{backgroundColor: "#F5F5F5", paddingVertical: 8, paddingHorizontal: 10, borderRadius: 10}}>
                  <Text style={{fontSize: 11, color: "#393939"}}>{item.challanId}</Text>
                </View>

                <View style={{flexDirection: "row", alignItems: "center"}}>
                  <TouchableOpacity style={{flexDirection: "row", alignItems: "center", marginRight: 10}}>
                    <Text style={{color: "#1A9E75", fontSize: 12}}>More Info</Text>
                    <Ionicons name="chevron-down" size={14} color="#1A9E75" />
                  </TouchableOpacity>

                  <TouchableOpacity style={{backgroundColor: "#1A9E75", paddingVertical: 5, paddingHorizontal: 11, borderRadius: 8}}>
                    <Text style={{color: "#F0FFFA", fontFamily:"Medium", fontSize: 10}}>Pay Now</Text>
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
    width: "90%", // Responsive width
    marginTop: 15,
    alignSelf: "center",
    marginBottom: 5,
    elevation: 5, // Android shadow
    shadowColor: "#000", // iOS shadow
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.2,
    shadowRadius: 4,
    backgroundColor: "#0BC189",
  },
  fullWidthTextContainer: {
    width: "100%", // Ensures full width
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
    paddingTop: 40, // Adjust top padding if needed
  },
  image: {
    width: 70,
    height: 90,
    resizeMode: "contain",
  },

});





































import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView, Modal, FlatList,Image,StyleSheet,Share,StatusBar   } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { LinearGradient } from 'expo-linear-gradient';
import * as Font from "expo-font";
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
  const [expanded, setExpanded] = useState(null);
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

  const toggleExpand = (index) => {
    setExpanded(expanded === index ? null : index);
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
      setErrorMessage(""); // ✅ Remove error when valid
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
  <TouchableOpacity onPress={() => navigation.goBack()} style={{ position: "absolute", left: 17 }}>
    <Ionicons name="arrow-back" size={24} color="white" />
  </TouchableOpacity>
  <Text 
    style={{ 
      fontSize: 16, 
      fontWeight: "700", 
      color: "#FFFBFB", 
      fontFamily: 'Poppins',
      marginLeft: 56, // Increased left margin to push text right
    }}
  >
    Pay Challan
  </Text>
</View>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 15, marginTop: 17, elevation: 2, marginHorizontal: 20,paddingBottom: 20,paddingTop: 11 }}>
        <Text style={{ fontSize: 16, fontWeight: "medium", marginBottom: 5,fontFamily: 'Poppins' }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8,fontWeight: "Normal",lineHeight:20  }}>Enter your vehicle number and check {"\n"}your challans</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5, borderRadius: 8 }}>
    <TextInput
      placeholder="Enter Your Vehicle Number"
      value={vehicleNumber}
      onChangeText={validateInput}
      maxLength={10}
      placeholderTextColor="#1A9E75"
      style={{
        width: 190, // Set fixed width instead of flex
        height: 40,
        backgroundColor: "#E6F9F0",
        borderWidth: 1.5,
        borderColor: isValid ? "#1A9E75" : "red",
        borderColor: "#1A9E75",
        borderRadius: 8,
        paddingHorizontal: 8,
        color: "#1A9E75",
        fontWeight: "bold",
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
      <Text style={{ color: "white", fontWeight: "bold" }}>Check</Text>
    </TouchableOpacity>
  </View>
   {/* Error Message (Only show if there's an error) */}
   {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5,alignSelf: "flex-start" }}>
            {errorMessage}
          </Text>
        )}
</View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Normal", marginBottom: 8,marginTop: -5 }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        {/* Step 1 */}
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
    {/* Outer Rounded Rectangle (Main Plate) */}
    <Rect x="10" y="20" width="160" height="60" rx="10" fill="#1A9E75" />

    {/* Inner White Rectangle (Text Background) */}
    <Rect x="20" y="30" width="140" height="40" rx="6" fill="white" />

    {/* Small Circles (Decorative Bolts) */}
    <Circle cx="28" cy="36" r="4" fill="#1A9E75" />
    <Circle cx="152" cy="36" r="4" fill="#1A9E75" />
    <Circle cx="28" cy="62" r="4" fill="#1A9E75" />
    <Circle cx="152" cy="62" r="4" fill="#1A9E75" />

    {/* Text (Number Plate Content) */}
    <SvgText x="90" y="55" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
    








            {/* <MaterialCommunityIcons name="license-plate" size={40} color="#1A9E75" /> */}
            {/* <Ionicons name="car-outline" size={30} color="#1A9E75" /> */}
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Enter your {"\n"} vehicle number</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        {/* Step 2 */}
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
    {/* Document Shape with Right-Bottom Cut */}
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />

    {/* Folded Corner */}
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    {/* Text Lines */}
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    {/* Challan Label */}
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
            {/* <Ionicons name="document-text-outline" size={30} color="#1A9E75" /> */}
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Check Your {"\n"} Challans</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        {/* Step 3 */}
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
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 3</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14 }}>Make {"\n"} Payment</Text>
        </View>
      </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, marginTop: 3, elevation: 2,width: "90%",marginHorizontal: 20 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8 }}>Traffic Fines</Text>{/* ,color:"#393939"*/}
        <Text style={{fontSize: 14,
                      color: "#AFAFAF",
                      marginBottom: 8,marginTop: -3}}>Select your state to see traffic fines</Text>
        
      

      {/* drop down state */}
      

  
      
      {/* Button to Open Modal */}
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
                        marginTop: 25, // 🔥 Decreased top space
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
      fontFamily: "Arial",
      
    }}>
      Invite your friends to make hassle-free
    </Text>
  </View>

      {/* Main Content - Text and Image */}
      <View style={{flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",}}>
        {/* Left Section - Text Content */}
        <View style={{flex: 1,
                      paddingRight: 10,}}>
          <Text style={{color: "white",
    fontSize: 18,
    fontFamily:"Poppins",
    fontWeight: "bold",
    marginBottom: 5,marginLeft:15}}>Challan Payments</Text>
          <Text style={{color: "white",
    fontSize: 10,
    marginBottom: 10,
    marginLeft:15}}>Pay using UPI, Debit & Credit Cards</Text>

          {/* Invite Now Button */}
          <TouchableOpacity style={{backgroundColor: "white",
                                    paddingVertical: 6,
                                    paddingHorizontal: 10,
                                    borderRadius: 15,
                                    alignItems: "center",
                                    alignSelf: "flex-start",marginLeft:15}} 
                                    onPress={shareReferral}>
          <Text style={{alignItems: "flex-end",fontSize:10,fontWeight:"bold",fontFamily:"Poppins"}}>Invite Now</Text>
        </TouchableOpacity>
        </View>

        {/* Right Section - Image */}
        <View style={{width: 80,
                      height: 100,
                      alignItems: "center",
                      justifyContent: "center",
                      borderBottomRightRadius: 1, 
                      paddingTop: 28, 
                      
                       }}>
          <Image
            source={require("../../assets/images/mega1.jpg")}  //mega1.jpg
            style={{width: 120,
              height: 90,
              resizeMode: "contain",}}
          />
        </View>
      </View>
      
    </LinearGradient>
       
       <View style={{marginTop: 15,marginHorizontal:20 }}>
        <Text style={{fontSize: 18, fontWeight: "bold", color: "#333", marginBottom: 10}}>FAQs</Text>
        {faqs.map((faq, index) => (
          <View key={index} style={{backgroundColor: "#FFFFFF", borderRadius: 10, marginBottom: 10, padding: 10, elevation: 2}}>
            <TouchableOpacity style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center", paddingVertical: 10 }} onPress={() => toggleFAQ(index)}>
              <Text style={{fontSize: 15, fontWeight: "600", color: "#333", fontFamily: "Calibri",lineHeight:20}}>{faq.question}</Text>
              <Ionicons name={faq.collapsed ? "chevron-down" : "chevron-up"} size={20} color="#333" />
            </TouchableOpacity>
            <Collapsible collapsed={faq.collapsed}>
              <Text style={{fontSize: 16, color: "#A0A0A0", paddingTop: 4, paddingHorizontal: 5,lineHeight: 18,fontFamily: "Poppins-Regular"}}>{faq.answer}</Text>
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
  const [expandedId, setExpandedId] = useState(null);
  const finesData = [
    { id: "1", title: "Carrying Excess \nLuggage", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "2", title: "Driving Without a \nNumber Plate", amount: "₹ 500",details:"	For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "3", title: "Parking in \nNo-parking Zone", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs.1500" },
    { id: "4", title: "Failure to Intimate Change\nof the Address \nof Vehicle Owner", amount: "₹ 500",details:"For the first offence Rs. 500, For repeat offence Rs. 1500" },

    { id: "5", title: "Driving Without a \nSeatbelt", amount: "₹ 1000" },
    { id: "6", title: "Triple Riding on \nTwo-vehicle", amount: "₹ 1000" },
    { id: "7", title: "Driving Without \nHelmet", amount: "₹ 1000" },
    { id: "8", title: "Over-speeding", amount: "₹ 1000",details:"For Light motor vehicle (LMV) Rs. 1000, For Medium passenger goods vehicle Rs. 2000" },
    { id: "9", title: "Violation of Road \nRegulations", amount: "₹ 1000" },
    { id: "10", title: "Driving When Mentally \nor Physically Unfit to Drive", amount: "₹ 1000",details:"For first offence Rs. 1000, For repeat offence Rs. 2000" },
     { id: "11", title: "Driving Without \nInsurance", amount: "₹ 2000" },
    { id: "12", title: "Overloading", amount: "₹ 2000" },
    { id: "13", title: "Driving Uninsured Vehicle", amount: "₹ 2000" },
     { id: "14", title: "Driving Vehicle \nWithout Registration", amount: "₹ 2000" },

      { id: "15", title: "Dangerous/Rash \nDriving", amount: "₹ 5000",details:"For first offence Rs. 5000, For repeat offence Rs. 10000" },
    { id: "16", title: "Disobey of Traffic \nSignals", amount: "₹ 5000",details:"For first offence Rs. 5000, For repeat offence Rs. 10000" },
    { id: "17", title: "Using a Mobile \nPhone While Driving", amount: "₹ 5000",details:"For the first offence Rs. 5000, For repeat offence Rs. 10000" },
    { id: "18", title: "Racing", amount: "₹ 5000",details:"For the first offence Rs. 5000, For repeat offence Rs. 10000" },
    { id: "19", title: "Driving Without a \nValid Driving Licence", amount: "₹ 5000" },
    { id: "20", title: "Drunk Driving", amount: "₹ 10,000" },
    { id: "21", title: "Carrying Explosive/\nInflammable Substances", amount: "₹ 10,000" },
    { id: "22", title: "Not Giving Passage \nto Emergency Vehicles", amount: "₹ 10,000" },
    { id: "23", title: "Disqualified Person \nDriving a Vehicle", amount: "₹ 10,000" },
     { id: "24", title: "Minor Driving \nVehicle", amount: "₹ 25,000" },
    // { id: "25", title: "Driving a Vehicle \nRegistered in Another \nState for More Than 12 Months", amount: "₹ 500" },
  ];

  return (
    <View style={{ flex: 1, backgroundColor: "#FFFFFF" }}>
      {/* Header */}
      <View style={{ backgroundColor: "#1A9E75", height: 88, flexDirection: "row", alignItems: "center", paddingHorizontal: 13, paddingTop: 25 }}>
        <TouchableOpacity onPress={() => navigation.goBack()}>
          <Ionicons name="arrow-back" size={24} color="white" />
        </TouchableOpacity>
        <Text style={{ fontSize: 16, fontWeight: "700", color: "#FFFBFB", marginLeft: 12,fontFamily:"Poppins" }}>
          Traffic Fines
        </Text>
      </View>

    {/* Select your State in Fines */}


    <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 10,
          
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#EEEEEE",
          borderWidth:1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          width: "91%",
          marginLeft:20,
          marginTop:20,
        }}
      >
        <Text style={{ color:"#AFAFAF", fontSize: 14,marginLeft:10,fontFamily:"Poppins" }}>
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
          backgroundColor: "#F0FFFA", // Light greenish background
          padding: 9, // Increased padding for better spacing
          borderRadius: 12,
          width: "91%", // Increased width to make it bigger
          alignSelf: "center", // Centers the container
          marginTop: 10, 
        }}>
  <MaterialCommunityIcons name="traffic-light" size={44} color="#1A9E75" paddingHorizontal="5" />
  
  {/* Text Container */}
  <View style={{ flex: 1, marginLeft: 1 }}>  
    <Text style={{
      fontSize: 16, // Slightly larger font
      fontWeight: "bold",

      color: "#333",
      fontFamily: "Poppins_600SemiBold",
      
    }}>
      Traffic fines in Maharashtra 
    </Text>
    
    <Text style={{
      fontSize: 8, // Slightly larger for better readability
      color: "#666",
      marginTop: 5,
      fontFamily: "Poppins_400Regular",
    }}>
      List of traffic fines as per Motor Vehicle (Amendment) Act
    </Text>
  </View>
</View>


<View style={{ flex: 1, backgroundColor: "#FFFFFF", padding: 10 }}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        contentContainerStyle={{ paddingBottom: 20 }}
        renderItem={({ item }) => {
          const isExpanded = expandedId === item.id;
          return (
            <View
              style={{
                backgroundColor: "#FFFFFF",
                borderRadius: 15,
                marginBottom: 10,
                elevation: 3,
                width: "96%",
                alignSelf: "center",
                paddingHorizontal: 15,
                paddingVertical: 10,
                borderColor: "#E4E4E4",
                borderWidth: 1,
              }}
            >
              <TouchableOpacity
                onPress={() =>
                  setExpandedId(isExpanded ? null : item.id)
                }
                style={{
                  flexDirection: "row",
                  alignItems: "center",
                  justifyContent: "space-between",
                  position: "relative",
                  
                }}
              >
                <Text
                  style={{
                    fontSize: 14,
                    color: "#393939",
                    flex: 1,
                    fontFamily:"Poppins",
                    textAlign: "left",
                    lineHeight: 20,
                  }}
                >
                  {item.title}
                </Text>

                 {/* Vertical Separator */}
                 <View
                      style={{
                        height: 27,
                        width: 1,
                        backgroundColor: "#E4E4E4",
                        position: "absolute",
                        left: 175, // Adjust this value as needed for proper alignment
                        top: "50%",
                        transform: [{ translateY: -13.5 }], // Centers vertically
                      }}
                    />

                    <View style={{ flex: 1, alignItems: "center",justifyContent: "center", }}>
                        <Text
                          style={{
                            fontSize: 14,
                            fontWeight: "600",
                            color: "#1A9E75",
                            textAlign: "center",
                            marginLeft:66,
                          }}
                        >
                          {item.amount}
                        </Text>
                      </View>
                <View style={{ flex: 0.2,
                                alignItems: "flex-end"}}>
                {item.details && (
                <Ionicons
                  name={isExpanded ? "chevron-up" : "chevron-down"}
                  size={20}
                  color="#393939"

                />
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
                  <Text style={{ fontSize: 15, color: "#A0A0A0",marginLeft:15 }}>
                    {item.details}
                  </Text>
                </View>
              )}
              </View>
              )}}
              />
              </View>
              </View>
  )
}


  const ChallanDetailsScreen = ({ navigation }) => {
    const [activeTab, setActiveTab] = useState("Pending");
  
    return (
      <View style={{ flex: 1, backgroundColor: "#fff" }}>
        {/* Header */}
        <View style={{ backgroundColor: "#1A9E75", height: 80, flexDirection: "row", alignItems: "center", paddingHorizontal: 16, paddingTop: 30 }}>
          <TouchableOpacity onPress={() => navigation.goBack()}>
            <Ionicons name="arrow-back" size={24} color="white" />
          </TouchableOpacity>
          <Text style={{ fontSize: 18, fontWeight: "700", color: "white", marginLeft: 16 }}>Challan Details</Text>
        </View>
  
        {/* Tabs */}
        <View style={{ flexDirection: "row", justifyContent: "space-around", paddingHorizontal:18,marginTop: 10,borderBottomWidth: 1, borderColor: "#ddd",marginLeft:10,marginRight:10 }}>
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Pending" ? "#1A9E75" : "#666" }}>Pending</Text>
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
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Paid" ? "#1A9E75" : "#666" }}>Paid</Text>
          </TouchableOpacity>
          
        </View>
  
        {/* Challan List */}
        {activeTab === "Pending" ? (
          <FlatList
            data={challans}
            keyExtractor={(item) => item.id}
            renderItem={({ item }) => (
              <View style={{backgroundColor: "white", margin: 10, padding: 15, borderRadius: 15, elevation: 2}}>
      
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#000"}}>{item.vehicleNumber}</Text>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#1A9E75"}}>
                {item.amount} 
                </Text>
              </View>

              {/* Due Date */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                {/* Due Date */}
                <Text style={{ fontSize: 14, color: "#888", flex: 1 }}>
                  Due Date: {item.dueDate}
                </Text>
        
                <LinearGradient 
                colors={["#E44E2D", "#E4852E"]} // Red-Orange Gradient
                start={{ x: 0, y: 0 }} 
                end={{ x: 1, y: 0 }} 
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
              <View style={{height: 1, backgroundColor: "#ddd", marginVertical: 8 }} />

              {/* Challan ID & Buttons */}
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <View style={{backgroundColor: "#F5F5F5", paddingVertical: 8, paddingHorizontal: 10, borderRadius: 10}}>
                  <Text style={{fontSize: 11, color: "#393939"}}>{item.challanId}</Text>
                </View>

                <View style={{flexDirection: "row", alignItems: "center"}}>
                  <TouchableOpacity style={{flexDirection: "row", alignItems: "center", marginRight: 10}}>
                    <Text style={{color: "#1A9E75", fontSize: 12}}>More Info</Text>
                    <Ionicons name="chevron-down" size={14} color="#1A9E75" />
                  </TouchableOpacity>

                  <TouchableOpacity style={{backgroundColor: "#1A9E75", paddingVertical: 5, paddingHorizontal: 11, borderRadius: 8}}>
                    <Text style={{color: "#F0FFFA", fontFamily:"Medium", fontSize: 10}}>Pay Now</Text>
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
    width: "90%", // Responsive width
    marginTop: 15,
    alignSelf: "center",
    marginBottom: 5,
    elevation: 5, // Android shadow
    shadowColor: "#000", // iOS shadow
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.2,
    shadowRadius: 4,
    backgroundColor: "#0BC189",
  },
  fullWidthTextContainer: {
    width: "100%", // Ensures full width
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
    paddingTop: 40, // Adjust top padding if needed
  },
  image: {
    width: 70,
    height: 90,
    resizeMode: "contain",
  },

});
























import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView, Modal, FlatList,Image,StyleSheet,Share,StatusBar   } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { LinearGradient } from 'expo-linear-gradient';
import * as Font from "expo-font";
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
  const [expanded, setExpanded] = useState(null);
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

  const toggleExpand = (index) => {
    setExpanded(expanded === index ? null : index);
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
      setErrorMessage(""); // ✅ Remove error when valid
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
  <TouchableOpacity onPress={() => navigation.goBack()} style={{ position: "absolute", left: 17 }}>
    <Ionicons name="arrow-back" size={24} color="white" />
  </TouchableOpacity>
  <Text 
    style={{ 
      fontSize: 16, 
      fontWeight: "700", 
      color: "#FFFBFB", 
      fontFamily: 'Poppins',
      marginLeft: 56, // Increased left margin to push text right
    }}
  >
    Pay Challan
  </Text>
</View>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 15, marginTop: 17, elevation: 2, marginHorizontal: 20,paddingBottom: 20,paddingTop: 11 }}>
        <Text style={{ fontSize: 16, fontWeight: "medium", marginBottom: 5,fontFamily: 'Poppins' }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8,fontWeight: "Normal",lineHeight:20  }}>Enter your vehicle number and check {"\n"}your challans</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5, borderRadius: 8 }}>
    <TextInput
      placeholder="Enter Your Vehicle Number"
      value={vehicleNumber}
      onChangeText={validateInput}
      maxLength={10}
      placeholderTextColor="#1A9E75"
      style={{
        width: 190, // Set fixed width instead of flex
        height: 40,
        backgroundColor: "#E6F9F0",
        borderWidth: 1.5,
        borderColor: isValid ? "#1A9E75" : "red",
        borderColor: "#1A9E75",
        borderRadius: 8,
        paddingHorizontal: 8,
        color: "#1A9E75",
        fontWeight: "bold",
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
      <Text style={{ color: "white", fontWeight: "bold" }}>Check</Text>
    </TouchableOpacity>
  </View>
   {/* Error Message (Only show if there's an error) */}
   {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5,alignSelf: "flex-start" }}>
            {errorMessage}
          </Text>
        )}
</View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Normal", marginBottom: 8,marginTop: -5 }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        {/* Step 1 */}
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
    {/* Outer Rounded Rectangle (Main Plate) */}
    <Rect x="10" y="20" width="160" height="60" rx="10" fill="#1A9E75" />

    {/* Inner White Rectangle (Text Background) */}
    <Rect x="20" y="30" width="140" height="40" rx="6" fill="white" />

    {/* Small Circles (Decorative Bolts) */}
    <Circle cx="28" cy="36" r="4" fill="#1A9E75" />
    <Circle cx="152" cy="36" r="4" fill="#1A9E75" />
    <Circle cx="28" cy="62" r="4" fill="#1A9E75" />
    <Circle cx="152" cy="62" r="4" fill="#1A9E75" />

    {/* Text (Number Plate Content) */}
    <SvgText x="90" y="55" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
    








            {/* <MaterialCommunityIcons name="license-plate" size={40} color="#1A9E75" /> */}
            {/* <Ionicons name="car-outline" size={30} color="#1A9E75" /> */}
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Enter your {"\n"} vehicle number</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        {/* Step 2 */}
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
    {/* Document Shape with Right-Bottom Cut */}
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />

    {/* Folded Corner */}
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    {/* Text Lines */}
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    {/* Challan Label */}
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
            {/* <Ionicons name="document-text-outline" size={30} color="#1A9E75" /> */}
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Check Your {"\n"} Challans</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        {/* Step 3 */}
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
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 3</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14 }}>Make {"\n"} Payment</Text>
        </View>
      </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, marginTop: 3, elevation: 2,width: "90%",marginHorizontal: 20 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8 }}>Traffic Fines</Text>{/* ,color:"#393939"*/}
        <Text style={{fontSize: 14,
                      color: "#AFAFAF",
                      marginBottom: 8,marginTop: -3}}>Select your state to see traffic fines</Text>
        
      

      {/* drop down state */}
      

  
      
      {/* Button to Open Modal */}
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
                        marginTop: 25, // 🔥 Decreased top space
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
      fontFamily: "Arial",
      
    }}>
      Invite your friends to make hassle-free
    </Text>
  </View>

      {/* Main Content - Text and Image */}
      <View style={{flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",}}>
        {/* Left Section - Text Content */}
        <View style={{flex: 1,
                      paddingRight: 10,}}>
          <Text style={{color: "white",
    fontSize: 18,
    fontFamily:"Poppins",
    fontWeight: "bold",
    marginBottom: 5,marginLeft:15}}>Challan Payments</Text>
          <Text style={{color: "white",
    fontSize: 10,
    marginBottom: 10,
    marginLeft:15}}>Pay using UPI, Debit & Credit Cards</Text>

          {/* Invite Now Button */}
          <TouchableOpacity style={{backgroundColor: "white",
                                    paddingVertical: 6,
                                    paddingHorizontal: 10,
                                    borderRadius: 15,
                                    alignItems: "center",
                                    alignSelf: "flex-start",marginLeft:15}} 
                                    onPress={shareReferral}>
          <Text style={{alignItems: "flex-end",fontSize:10,fontWeight:"bold",fontFamily:"Poppins"}}>Invite Now</Text>
        </TouchableOpacity>
        </View>

        {/* Right Section - Image */}
        <View style={{width: 80,
                      height: 100,
                      alignItems: "center",
                      justifyContent: "center",
                      borderBottomRightRadius: 1, 
                      paddingTop: 28, 
                      
                       }}>
          <Image
            source={require("../../assets/images/mega1.jpg")}  //mega1.jpg
            style={{width: 120,
              height: 90,
              resizeMode: "contain",}}
          />
        </View>
      </View>
      
    </LinearGradient>
       
       <View style={{marginTop: 15,marginHorizontal:20 }}>
        <Text style={{fontSize: 18, fontWeight: "bold", color: "#333", marginBottom: 10}}>FAQs</Text>
        {faqs.map((faq, index) => (
          <View key={index} style={{backgroundColor: "#FFFFFF", borderRadius: 10, marginBottom: 10, padding: 10, elevation: 2}}>
            <TouchableOpacity style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center", paddingVertical: 10 }} onPress={() => toggleFAQ(index)}>
              <Text style={{fontSize: 15, fontWeight: "600", color: "#333", fontFamily: "Calibri",lineHeight:20}}>{faq.question}</Text>
              <Ionicons name={faq.collapsed ? "chevron-down" : "chevron-up"} size={20} color="#333" />
            </TouchableOpacity>
            <Collapsible collapsed={faq.collapsed}>
              <Text style={{fontSize: 16, color: "#A0A0A0", paddingTop: 4, paddingHorizontal: 5,lineHeight: 18,fontFamily: "Poppins-Regular"}}>{faq.answer}</Text>
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
  const [expandedId, setExpandedId] = useState(null);
  const finesData = [
    { id: "1", title: "Carrying Excess \nLuggage", amount: "₹ 500",details:"For first offence Rs. 500, For repeat offence Rs. 1500" },
    { id: "2", title: "Driving Without a \nNumber Plate", amount: "₹ 500",details:"	For first offence Rs. 500 For repeat offence Rs. 1500" },
    { id: "3", title: "Parking in \nNo-parking Zone", amount: "₹ 500",details:"For first offence Rs. 500 For repeat offence Rs.1500" },
    { id: "4", title: "Failure to Intimate \nChange of the Address \nof Vehicle Owner", amount: "₹ 500",details:"For the first offence Rs. 500 For repeat offence Rs. 1500" },

    { id: "5", title: "Driving Without a \nSeatbelt", amount: "₹ 1000" },
    { id: "6", title: "Triple Riding on \nTwo-vehicle", amount: "₹ 1000" },
    { id: "7", title: "Driving Without \nHelmet", amount: "₹ 1000" },
    { id: "8", title: "Over-speeding", amount: "₹ 1000",details:"For Light motor vehicle (LMV): ₹1000 For Medium passenger goods vehicle: ₹2000" },
    { id: "9", title: "Violation of Road \nRegulations", amount: "₹ 1000" },
    { id: "10", title: "Driving When Mentally \nor Physically Unfit to Drive", amount: "₹ 1000",details:"For first offence Rs. 1000 For repeat offence Rs. 2000" },
     { id: "11", title: "Driving Without \nInsurance", amount: "₹ 2000" },
    { id: "12", title: "Overloading", amount: "₹ 2000" },
    { id: "13", title: "Driving Uninsured Vehicle", amount: "₹ 2000" },
     { id: "14", title: "Driving Vehicle \nWithout Registration", amount: "₹ 2000" },

      { id: "15", title: "Dangerous/Rash \nDriving", amount: "₹ 5000",details:"For first offence Rs. 5000 For repeat offence Rs. 10000" },
    { id: "16", title: "Disobey of Traffic \nSignals", amount: "₹ 5000",details:"For first offence Rs. 5000 For repeat offence Rs. 10000" },
    { id: "17", title: "Using a Mobile \nPhone While Driving", amount: "₹ 5000",details:"For the first offence Rs. 5000 For repeat offence Rs. 10000" },
    { id: "18", title: "Racing", amount: "₹ 5000",details:"For the first offence Rs. 5000 For repeat offence Rs. 10000" },
    { id: "19", title: "Driving Without a \nValid Driving Licence", amount: "₹ 5000" },
    { id: "20", title: "Drunk Driving", amount: "₹ 10,000" },
    { id: "21", title: "Carrying Explosive/\nInflammable Substances", amount: "₹ 10,000" },
    { id: "22", title: "Not Giving Passage \nto Emergency Vehicles", amount: "₹ 10,000" },
    { id: "23", title: "Disqualified Person \nDriving a Vehicle", amount: "₹ 10,000" },
     { id: "24", title: "Minor Driving \nVehicle", amount: "₹ 25,000" },
    // { id: "25", title: "Driving a Vehicle \nRegistered in Another \nState for More Than 12 Months", amount: "₹ 500" },
  ];

  return (
    <View style={{ flex: 1, backgroundColor: "#FFFFFF" }}>
      {/* Header */}
      <View style={{ backgroundColor: "#1A9E75", height: 88, flexDirection: "row", alignItems: "center", paddingHorizontal: 13, paddingTop: 25 }}>
        <TouchableOpacity onPress={() => navigation.goBack()}>
          <Ionicons name="arrow-back" size={24} color="white" />
        </TouchableOpacity>
        <Text style={{ fontSize: 16, fontWeight: "700", color: "#FFFBFB", marginLeft: 12,fontFamily:"Poppins" }}>
          Traffic Fines
        </Text>
      </View>

    {/* Select your State in Fines */}


    <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 10,
          
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#EEEEEE",
          borderWidth:1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          width: "91%",
          marginLeft:20,
          marginTop:20,
        }}
      >
        <Text style={{ color:"#AFAFAF", fontSize: 14,marginLeft:10,fontFamily:"Poppins" }}>
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
          backgroundColor: "#F0FFFA", // Light greenish background
          padding: 9, // Increased padding for better spacing
          borderRadius: 12,
          width: "91%", // Increased width to make it bigger
          alignSelf: "center", // Centers the container
          marginTop: 10, 
        }}>
  <MaterialCommunityIcons name="traffic-light" size={44} color="#1A9E75" paddingHorizontal="5" />
  
  {/* Text Container */}
  <View style={{ flex: 1, marginLeft: 1 }}>  
    <Text style={{
      fontSize: 16, // Slightly larger font
      fontWeight: "bold",

      color: "#333",
      fontFamily: "Poppins_600SemiBold",
      
    }}>
      Traffic fines in Maharashtra 
    </Text>
    
    <Text style={{
      fontSize: 8, // Slightly larger for better readability
      color: "#666",
      marginTop: 5,
      fontFamily: "Poppins_400Regular",
    }}>
      List of traffic fines as per Motor Vehicle (Amendment) Act
    </Text>
  </View>
</View>


<View style={{ flex: 1, backgroundColor: "#FFFFFF", padding: 10 }}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        contentContainerStyle={{ paddingBottom: 20 }}
        renderItem={({ item }) => {
          const isExpanded = expandedId === item.id;
          return (
            <View
              style={{
                backgroundColor: "#FFFFFF",
                borderRadius: 15,
                marginBottom: 10,
                elevation: 3,
                width: "96%",
                alignSelf: "center",
                paddingHorizontal: 15,
                paddingVertical: 10,
                borderColor: "#E4E4E4",
                borderWidth: 1,
              }}
            >
              <TouchableOpacity
                onPress={() =>
                  setExpandedId(isExpanded ? null : item.id)
                }
                style={{
                  flexDirection: "row",
                  alignItems: "center",
                  justifyContent: "space-between",
                  position: "relative",
                  
                }}
              >
                <Text
                  style={{
                    fontSize: 14,
                    color: "#393939",
                    flex: 1,
                    textAlign: "left",
                    lineHeight: 20,
                  }}
                >
                  {item.title}
                </Text>

                 {/* Vertical Separator */}
                 <View
                      style={{
                        height: 27,
                        width: 1,
                        backgroundColor: "#E4E4E4",
                        position: "absolute",
                        left: 175, // Adjust this value as needed for proper alignment
                        top: "50%",
                        transform: [{ translateY: -13.5 }], // Centers vertically
                      }}
                    />

                    <View style={{ flex: 1, alignItems: "center" }}>
                        <Text
                          style={{
                            fontSize: 14,
                            fontWeight: "600",
                            color: "#1A9E75",
                            textAlign: "center",
                            marginLeft:46,
                          }}
                        >
                          {item.amount}
                        </Text>
                      </View>
                
                {item.details && (
                <Ionicons
                  name={isExpanded ? "chevron-up" : "chevron-down"}
                  size={20}
                  color="#393939"
                />
                )}
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
                  <Text style={{ fontSize: 15, color: "#A0A0A0",marginLeft:15 }}>
                    {item.details}
                  </Text>
                </View>
              )}
              </View>
              )}}
              />
              </View>
              </View>
  )
}


  const ChallanDetailsScreen = ({ navigation }) => {
    const [activeTab, setActiveTab] = useState("Pending");
  
    return (
      <View style={{ flex: 1, backgroundColor: "#fff" }}>
        {/* Header */}
        <View style={{ backgroundColor: "#1A9E75", height: 80, flexDirection: "row", alignItems: "center", paddingHorizontal: 16, paddingTop: 30 }}>
          <TouchableOpacity onPress={() => navigation.goBack()}>
            <Ionicons name="arrow-back" size={24} color="white" />
          </TouchableOpacity>
          <Text style={{ fontSize: 18, fontWeight: "700", color: "white", marginLeft: 16 }}>Challan Details</Text>
        </View>
  
        {/* Tabs */}
        <View style={{ flexDirection: "row", justifyContent: "space-around", paddingHorizontal:18,marginTop: 10,borderBottomWidth: 1, borderColor: "#ddd",marginLeft:10,marginRight:10 }}>
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Pending" ? "#1A9E75" : "#666" }}>Pending</Text>
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
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Paid" ? "#1A9E75" : "#666" }}>Paid</Text>
          </TouchableOpacity>
          
        </View>
  
        {/* Challan List */}
        {activeTab === "Pending" ? (
          <FlatList
            data={challans}
            keyExtractor={(item) => item.id}
            renderItem={({ item }) => (
              <View style={{backgroundColor: "white", margin: 10, padding: 15, borderRadius: 15, elevation: 2}}>
      
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#000"}}>{item.vehicleNumber}</Text>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#1A9E75"}}>
                {item.amount} 
                </Text>
              </View>

              {/* Due Date */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                {/* Due Date */}
                <Text style={{ fontSize: 14, color: "#888", flex: 1 }}>
                  Due Date: {item.dueDate}
                </Text>
        
                <LinearGradient 
                colors={["#E44E2D", "#E4852E"]} // Red-Orange Gradient
                start={{ x: 0, y: 0 }} 
                end={{ x: 1, y: 0 }} 
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
              <View style={{height: 1, backgroundColor: "#ddd", marginVertical: 8 }} />

              {/* Challan ID & Buttons */}
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <View style={{backgroundColor: "#F5F5F5", paddingVertical: 8, paddingHorizontal: 10, borderRadius: 10}}>
                  <Text style={{fontSize: 11, color: "#393939"}}>{item.challanId}</Text>
                </View>

                <View style={{flexDirection: "row", alignItems: "center"}}>
                  <TouchableOpacity style={{flexDirection: "row", alignItems: "center", marginRight: 10}}>
                    <Text style={{color: "#1A9E75", fontSize: 12}}>More Info</Text>
                    <Ionicons name="chevron-down" size={14} color="#1A9E75" />
                  </TouchableOpacity>

                  <TouchableOpacity style={{backgroundColor: "#1A9E75", paddingVertical: 5, paddingHorizontal: 11, borderRadius: 8}}>
                    <Text style={{color: "#F0FFFA", fontFamily:"Medium", fontSize: 10}}>Pay Now</Text>
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
    width: "90%", // Responsive width
    marginTop: 15,
    alignSelf: "center",
    marginBottom: 5,
    elevation: 5, // Android shadow
    shadowColor: "#000", // iOS shadow
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.2,
    shadowRadius: 4,
    backgroundColor: "#0BC189",
  },
  fullWidthTextContainer: {
    width: "100%", // Ensures full width
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
    paddingTop: 40, // Adjust top padding if needed
  },
  image: {
    width: 70,
    height: 90,
    resizeMode: "contain",
  },

});
































import React from "react";
import { View, Text, FlatList, StyleSheet } from "react-native";

const finesData = [
  { id: "1", description: "Violation of general traffic rules", amount: "₹ 500" },
  { id: "2", description: "Violation of road regulations", amount: "₹ 500" },
  { id: "3", description: "Traveling without ticket", amount: "₹ 200 to 500" },
  { id: "4", description: "Disobeying orders of traffic police", amount: "₹ 2000" },
  { id: "5", description: "Driving without valid Registration Certificate", amount: "₹ 5000" },
  { id: "6", description: "Driving without valid Driving License", amount: "₹ 5000" },
];

const TrafficFinesScreen = () => {
  const renderItem = ({ item }) => (
    <View style={styles.card}>
      {/* Description */}
      <View style={styles.descriptionContainer}>
        <Text style={styles.description}>{item.description}</Text>
      </View>

      {/* Separator */}
      <View style={styles.separator} />

      {/* Amount */}
      <View style={styles.amountContainer}>
        <Text style={styles.amount}>{item.amount}</Text>
      </View>
    </View>
  );

  return (
    <View style={styles.container}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        renderItem={renderItem}
      />
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#F8F8F8",
    padding: 10,
  },
  card: {
    flexDirection: "row",
    backgroundColor: "white",
    padding: 15,
    borderRadius: 10,
    marginBottom: 10,
    shadowColor: "#000",
    shadowOpacity: 0.1,
    shadowRadius: 5,
    shadowOffset: { width: 0, height: 2 },
    elevation: 2,
    alignItems: "center",
  },
  descriptionContainer: {
    flex: 3,
  },
  description: {
    fontSize: 14,
    fontWeight: "500",
    color: "#333",
  },
  separator: {
    height: 27,
    width: 1,
    backgroundColor: "#E4E4E4",
    marginHorizontal: 10,
  },
  amountContainer: {
    flex: 1,
    alignItems: "center",
  },
  amount: {
    fontSize: 14,
    fontWeight: "600",
    color: "#1A9E75",
    textAlign: "center",
  },
});

export default TrafficFinesScreen;






























///Correct code-wednesday

import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView, Modal, FlatList,Image,StyleSheet,Share,StatusBar   } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { LinearGradient } from 'expo-linear-gradient';
import * as Font from "expo-font";
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
  const [expanded, setExpanded] = useState(null);
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

  const toggleExpand = (index) => {
    setExpanded(expanded === index ? null : index);
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
      setErrorMessage(""); // ✅ Remove error when valid
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
  <TouchableOpacity onPress={() => navigation.goBack()} style={{ position: "absolute", left: 17 }}>
    <Ionicons name="arrow-back" size={24} color="white" />
  </TouchableOpacity>
  <Text 
    style={{ 
      fontSize: 16, 
      fontWeight: "700", 
      color: "#FFFBFB", 
      fontFamily: 'Poppins',
      marginLeft: 56, // Increased left margin to push text right
    }}
  >
    Pay Challan
  </Text>
</View>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 15, marginTop: 17, elevation: 2, marginHorizontal: 20,paddingBottom: 20,paddingTop: 11 }}>
        <Text style={{ fontSize: 16, fontWeight: "medium", marginBottom: 5,fontFamily: 'Poppins' }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8,fontWeight: "Normal",lineHeight:20  }}>Enter your vehicle number and check {"\n"}your challans</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5, borderRadius: 8 }}>
    <TextInput
      placeholder="Enter Your Vehicle Number"
      value={vehicleNumber}
      onChangeText={validateInput}
      maxLength={10}
      placeholderTextColor="#1A9E75"
      style={{
        width: 190, // Set fixed width instead of flex
        height: 40,
        backgroundColor: "#E6F9F0",
        borderWidth: 1.5,
        borderColor: isValid ? "#1A9E75" : "red",
        borderColor: "#1A9E75",
        borderRadius: 8,
        paddingHorizontal: 8,
        color: "#1A9E75",
        fontWeight: "bold",
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
      <Text style={{ color: "white", fontWeight: "bold" }}>Check</Text>
    </TouchableOpacity>
  </View>
   {/* Error Message (Only show if there's an error) */}
   {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5,alignSelf: "flex-start" }}>
            {errorMessage}
          </Text>
        )}
</View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Normal", marginBottom: 8,marginTop: -5 }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        {/* Step 1 */}
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
    {/* Outer Rounded Rectangle (Main Plate) */}
    <Rect x="10" y="20" width="160" height="60" rx="10" fill="#1A9E75" />

    {/* Inner White Rectangle (Text Background) */}
    <Rect x="20" y="30" width="140" height="40" rx="6" fill="white" />

    {/* Small Circles (Decorative Bolts) */}
    <Circle cx="28" cy="36" r="4" fill="#1A9E75" />
    <Circle cx="152" cy="36" r="4" fill="#1A9E75" />
    <Circle cx="28" cy="62" r="4" fill="#1A9E75" />
    <Circle cx="152" cy="62" r="4" fill="#1A9E75" />

    {/* Text (Number Plate Content) */}
    <SvgText x="90" y="55" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
    








            {/* <MaterialCommunityIcons name="license-plate" size={40} color="#1A9E75" /> */}
            {/* <Ionicons name="car-outline" size={30} color="#1A9E75" /> */}
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Enter your {"\n"} vehicle number</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        {/* Step 2 */}
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
    {/* Document Shape with Right-Bottom Cut */}
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />

    {/* Folded Corner */}
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    {/* Text Lines */}
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    {/* Challan Label */}
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
            {/* <Ionicons name="document-text-outline" size={30} color="#1A9E75" /> */}
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Check Your {"\n"} Challans</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        {/* Step 3 */}
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
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 3</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14 }}>Make {"\n"} Payment</Text>
        </View>
      </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, marginTop: 3, elevation: 2,width: "90%",marginHorizontal: 20 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8 }}>Traffic Fines</Text>{/* ,color:"#393939"*/}
        <Text style={{fontSize: 14,
                      color: "#AFAFAF",
                      marginBottom: 8,marginTop: -3}}>Select your state to see traffic fines</Text>
        
      

      {/* drop down state */}
      

  
      
      {/* Button to Open Modal */}
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
                        marginTop: 25, // 🔥 Decreased top space
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
      fontFamily: "Arial",
      
    }}>
      Invite your friends to make hassle-free
    </Text>
  </View>

      {/* Main Content - Text and Image */}
      <View style={{flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",}}>
        {/* Left Section - Text Content */}
        <View style={{flex: 1,
                      paddingRight: 10,}}>
          <Text style={{color: "white",
    fontSize: 18,
    fontFamily:"Poppins",
    fontWeight: "bold",
    marginBottom: 5,marginLeft:15}}>Challan Payments</Text>
          <Text style={{color: "white",
    fontSize: 10,
    marginBottom: 10,
    marginLeft:15}}>Pay using UPI, Debit & Credit Cards</Text>

          {/* Invite Now Button */}
          <TouchableOpacity style={{backgroundColor: "white",
                                    paddingVertical: 6,
                                    paddingHorizontal: 10,
                                    borderRadius: 15,
                                    alignItems: "center",
                                    alignSelf: "flex-start",marginLeft:15}} 
                                    onPress={shareReferral}>
          <Text style={{alignItems: "flex-end",fontSize:10,fontWeight:"bold",fontFamily:"Poppins"}}>Invite Now</Text>
        </TouchableOpacity>
        </View>

        {/* Right Section - Image */}
        <View style={{width: 80,
                      height: 100,
                      alignItems: "center",
                      justifyContent: "center",
                      borderBottomRightRadius: 1, 
                      paddingTop: 28, 
                      
                       }}>
          <Image
            source={require("../../assets/images/mega1.jpg")}  //mega1.jpg
            style={{width: 120,
              height: 90,
              resizeMode: "contain",}}
          />
        </View>
      </View>
      
    </LinearGradient>
       
       <View style={{marginTop: 15,marginHorizontal:20 }}>
        <Text style={{fontSize: 18, fontWeight: "bold", color: "#333", marginBottom: 10}}>FAQs</Text>
        {faqs.map((faq, index) => (
          <View key={index} style={{backgroundColor: "#FFFFFF", borderRadius: 10, marginBottom: 10, padding: 10, elevation: 2}}>
            <TouchableOpacity style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center", paddingVertical: 10 }} onPress={() => toggleFAQ(index)}>
              <Text style={{fontSize: 15, fontWeight: "600", color: "#333", fontFamily: "Calibri",lineHeight:20}}>{faq.question}</Text>
              <Ionicons name={faq.collapsed ? "chevron-down" : "chevron-up"} size={20} color="#333" />
            </TouchableOpacity>
            <Collapsible collapsed={faq.collapsed}>
              <Text style={{fontSize: 16, color: "#A0A0A0", paddingTop: 4, paddingHorizontal: 5,lineHeight: 18,fontFamily: "Poppins-Regular"}}>{faq.answer}</Text>
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
  const [expandedId, setExpandedId] = useState(null);
  const finesData = [
    { id: "1", title: "Driving Without a \nSeatbelt", amount: "₹ 1000" },
    { id: "2", title: "Carrying Excess \nLuggage", amount: "₹ 500",details:"For the first offence: Rs. 500 For repeat offence: Rs. 1500" },
    { id: "3", title: "Triple Riding on \nTwo-vehicle", amount: "₹ 1000" },
    { id: "4", title: "Driving Without a \nNumber Plate", amount: "₹ 500",details:"	For the first offence: ₹500 For repeat offence: ₹1500" },
    { id: "5", title: "Driving Without \nHelmet", amount: "₹ 1000" },
    { id: "6", title: "Minor Driving \nVehicle", amount: "₹ 25,000" },
    { id: "7", title: "Parking in \nNo-parking Zone", amount: "₹ 500",details:"For the first offence: ₹500 For repeat offence: ₹1500" },
    { id: "8", title: "Dangerous/Rash \nDriving", amount: "₹ 5000",details:"For the first offence: ₹5000 For repeat offence: ₹10000" },
    { id: "9", title: "Disobey of Traffic \nSignals", amount: "₹ 5000",details:"For first offence: ₹5000 For repeat offence: ₹10000" },
    { id: "10", title: "Using a Mobile \nPhone While Driving", amount: "₹ 5000",details:"For the first offence: ₹5000 For repeat offence: ₹10000" },
    { id: "11", title: "Driving Uninsured Vehicle", amount: "₹ 2000" },
    { id: "12", title: "Drunk Driving", amount: "₹ 10,000" },
    { id: "13", title: "Driving Vehicle \nWithout Registration", amount: "₹ 2000" },
    { id: "14", title: "Over-speeding", amount: "₹ 1000",details:"For Light motor vehicle (LMV): ₹1000 For Medium passenger goods vehicle: ₹2000" },
    { id: "15", title: "Carrying Explosive/\nInflammable Substances", amount: "₹ 10,000" },
    { id: "16", title: "Violation of Road \nRegulations", amount: "₹ 1000" },
    { id: "17", title: "Driving12 When Mentally \nor Physically Unfit to Drive", amount: "₹ 1000",details:"For the first offence: ₹1000 For repeat offence: ₹2000" },
    { id: "18", title: "Not Giving Passage \nto Emergency Vehicles", amount: "₹ 10,000" },
    { id: "19", title: "Disqualified Person \nDriving a Vehicle", amount: "₹ 10,000" },
    { id: "20", title: "Driving Without \nInsurance", amount: "₹ 2000" },
    { id: "21", title: "Overloading", amount: "₹ 2000" },
    { id: "22", title: "Racing", amount: "₹ 5000",details:"For the first offence: ₹5000 For repeat offence: ₹10000" },
    { id: "23", title: "Driving Without a \nValid Driving Licence", amount: "₹ 5000" },
    // { id: "25", title: "Driving a Vehicle \nRegistered in Another \nState for More Than 12 Months", amount: "₹ 500" },
    { id: "24", title: "Failure to Intimate \nChange of the Address \nof Vehicle Owner", amount: "₹ 500",details:"For the first offence: ₹500 For repeat offence: ₹1500" },
  ];

  return (
    <View style={{ flex: 1, backgroundColor: "#FFFFFF" }}>
      {/* Header */}
      <View style={{ backgroundColor: "#1A9E75", height: 88, flexDirection: "row", alignItems: "center", paddingHorizontal: 13, paddingTop: 25 }}>
        <TouchableOpacity onPress={() => navigation.goBack()}>
          <Ionicons name="arrow-back" size={24} color="white" />
        </TouchableOpacity>
        <Text style={{ fontSize: 16, fontWeight: "700", color: "#FFFBFB", marginLeft: 12,fontFamily:"Poppins" }}>
          Traffic Fines
        </Text>
      </View>

    {/* Select your State in Fines */}


    <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 10,
          
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#EEEEEE",
          borderWidth:1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          width: "91%",
          marginLeft:20,
          marginTop:20,
        }}
      >
        <Text style={{ color:"#AFAFAF", fontSize: 14,marginLeft:10,fontFamily:"Poppins" }}>
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
          backgroundColor: "#F0FFFA", // Light greenish background
          padding: 9, // Increased padding for better spacing
          borderRadius: 12,
          width: "91%", // Increased width to make it bigger
          alignSelf: "center", // Centers the container
          marginTop: 10, 
        }}>
  <MaterialCommunityIcons name="traffic-light" size={44} color="#1A9E75" paddingHorizontal="5" />
  
  {/* Text Container */}
  <View style={{ flex: 1, marginLeft: 1 }}>  
    <Text style={{
      fontSize: 16, // Slightly larger font
      fontWeight: "bold",

      color: "#333",
      fontFamily: "Poppins_600SemiBold",
      
    }}>
      Traffic fines in Maharashtra 
    </Text>
    
    <Text style={{
      fontSize: 8, // Slightly larger for better readability
      color: "#666",
      marginTop: 5,
      fontFamily: "Poppins_400Regular",
    }}>
      List of traffic fines as per Motor Vehicle (Amendment) Act
    </Text>
  </View>
</View>


<View style={{ flex: 1, backgroundColor: "#FFFFFF", padding: 10 }}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        contentContainerStyle={{ paddingBottom: 20 }}
        renderItem={({ item }) => {
          const isExpanded = expandedId === item.id;
          return (
            <View
              style={{
                backgroundColor: "#FFFFFF",
                borderRadius: 15,
                marginBottom: 10,
                elevation: 3,
                width: "96%",
                alignSelf: "center",
                paddingHorizontal: 15,
                paddingVertical: 10,
                borderColor: "#E4E4E4",
                borderWidth: 1,
              }}
            >
              <TouchableOpacity
                onPress={() =>
                  setExpandedId(isExpanded ? null : item.id)
                }
                style={{
                  flexDirection: "row",
                  alignItems: "center",
                  justifyContent: "space-between",
                  position: "relative",
                  
                }}
              >
                <Text
                  style={{
                    fontSize: 14,
                    color: "#393939",
                    flex: 1,
                    textAlign: "left",
                    lineHeight: 20,
                  }}
                >
                  {item.title}
                </Text>

                 {/* Vertical Separator */}
                 <View
                      style={{
                        height: 27,
                        width: 1,
                        backgroundColor: "#E4E4E4",
                        position: "absolute",
                        left: 175, // Adjust this value as needed for proper alignment
                        top: "50%",
                        transform: [{ translateY: -13.5 }], // Centers vertically
                      }}
                    />

                    <View style={{ flex: 1, alignItems: "center" }}>
                        <Text
                          style={{
                            fontSize: 14,
                            fontWeight: "600",
                            color: "#1A9E75",
                            textAlign: "center",
                            marginLeft:46,
                          }}
                        >
                          {item.amount}
                        </Text>
                      </View>

                <Ionicons
                  name={isExpanded ? "chevron-up" : "chevron-down"}
                  size={20}
                  color="#393939"
                />
              </TouchableOpacity>

              {isExpanded && (
                <View
                  style={{
                    backgroundColor: "white",
                    padding: 8,
                    borderRadius: 8,
                    marginTop: 5,
                  }}
                >
                  <Text style={{ fontSize: 12, color: "#A0A0A0" }}>
                    {item.details}
                  </Text>
                </View>
              )}
              </View>
              )}}
              />
              </View>
              </View>
  )
}


  const ChallanDetailsScreen = ({ navigation }) => {
    const [activeTab, setActiveTab] = useState("Pending");
  
    return (
      <View style={{ flex: 1, backgroundColor: "#fff" }}>
        {/* Header */}
        <View style={{ backgroundColor: "#1A9E75", height: 80, flexDirection: "row", alignItems: "center", paddingHorizontal: 16, paddingTop: 30 }}>
          <TouchableOpacity onPress={() => navigation.goBack()}>
            <Ionicons name="arrow-back" size={24} color="white" />
          </TouchableOpacity>
          <Text style={{ fontSize: 18, fontWeight: "700", color: "white", marginLeft: 16 }}>Challan Details</Text>
        </View>
  
        {/* Tabs */}
        <View style={{ flexDirection: "row", justifyContent: "space-around", paddingHorizontal:18,marginTop: 10,borderBottomWidth: 1, borderColor: "#ddd",marginLeft:10,marginRight:10 }}>
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Pending" ? "#1A9E75" : "#666" }}>Pending</Text>
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
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Paid" ? "#1A9E75" : "#666" }}>Paid</Text>
          </TouchableOpacity>
          
        </View>
  
        {/* Challan List */}
        {activeTab === "Pending" ? (
          <FlatList
            data={challans}
            keyExtractor={(item) => item.id}
            renderItem={({ item }) => (
              <View style={{backgroundColor: "white", margin: 10, padding: 15, borderRadius: 15, elevation: 2}}>
      
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#000"}}>{item.vehicleNumber}</Text>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#1A9E75"}}>
                {item.amount} 
                </Text>
              </View>

              {/* Due Date */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                {/* Due Date */}
                <Text style={{ fontSize: 14, color: "#888", flex: 1 }}>
                  Due Date: {item.dueDate}
                </Text>
        
                <LinearGradient 
                colors={["#E44E2D", "#E4852E"]} // Red-Orange Gradient
                start={{ x: 0, y: 0 }} 
                end={{ x: 1, y: 0 }} 
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
              <View style={{height: 1, backgroundColor: "#ddd", marginVertical: 8 }} />

              {/* Challan ID & Buttons */}
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <View style={{backgroundColor: "#F5F5F5", paddingVertical: 8, paddingHorizontal: 10, borderRadius: 10}}>
                  <Text style={{fontSize: 11, color: "#393939"}}>{item.challanId}</Text>
                </View>

                <View style={{flexDirection: "row", alignItems: "center"}}>
                  <TouchableOpacity style={{flexDirection: "row", alignItems: "center", marginRight: 10}}>
                    <Text style={{color: "#1A9E75", fontSize: 12}}>More Info</Text>
                    <Ionicons name="chevron-down" size={14} color="#1A9E75" />
                  </TouchableOpacity>

                  <TouchableOpacity style={{backgroundColor: "#1A9E75", paddingVertical: 5, paddingHorizontal: 11, borderRadius: 8}}>
                    <Text style={{color: "#F0FFFA", fontFamily:"Medium", fontSize: 10}}>Pay Now</Text>
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
    width: "90%", // Responsive width
    marginTop: 15,
    alignSelf: "center",
    marginBottom: 5,
    elevation: 5, // Android shadow
    shadowColor: "#000", // iOS shadow
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.2,
    shadowRadius: 4,
    backgroundColor: "#0BC189",
  },
  fullWidthTextContainer: {
    width: "100%", // Ensures full width
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
    paddingTop: 40, // Adjust top padding if needed
  },
  image: {
    width: 70,
    height: 90,
    resizeMode: "contain",
  },

});











































import React, { useState } from "react";
import { View, Text, FlatList, TouchableOpacity } from "react-native";
import { Ionicons } from "@expo/vector-icons";

const finesData = [
  { id: "1", title: "Not wearing seat belt", amount: "₹ 1000" },
  { id: "2", title: "Blocking emergency vehicles", amount: "₹ 10,000" },
  { id: "3", title: "Overloading of passengers", amount: "₹ 1000" },
  { id: "4", title: "Overspeeding", amount: "₹ 1000", details: "For LMV: ₹1000, for HMV: ₹2000" },
  { id: "5", title: "Riding a two-wheeler with more than two persons", amount: "₹ 2000", details: "Fine applies for triple riding" },
  { id: "6", title: "Riding without helmet", amount: "₹ 2000" },
];

const TrafficFineList = () => {
  const [expandedId, setExpandedId] = useState(null);

  return (
    <View style={{ flex: 1, backgroundColor: "#F8F9FA", padding: 15 }}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        contentContainerStyle={{ paddingBottom: 20 }}
        renderItem={({ item }) => {
          const isExpanded = expandedId === item.id;
          return (
            <View
              style={{
                backgroundColor: "#FFFFFF",
                borderRadius: 12,
                marginBottom: 10,
                paddingHorizontal: 15,
                paddingVertical: 12,
                shadowColor: "#000",
                shadowOpacity: 0.05,
                shadowRadius: 2,
                elevation: 2,
                flexDirection: "column",
              }}
            >
              <TouchableOpacity
                onPress={() => setExpandedId(isExpanded ? null : item.id)}
                style={{
                  flexDirection: "row",
                  alignItems: "center",
                  justifyContent: "space-between",
                }}
              >
                {/* Title Section */}
                <Text style={{ fontSize: 15, color: "#393939", flex: 1 }}>
                  {item.title}
                </Text>

                {/* Separator */}
                <View
                  style={{
                    height: 20,
                    width: 1,
                    backgroundColor: "#E4E4E4",
                    marginHorizontal: 15,
                  }}
                />

                {/* Amount Section */}
                <Text
                  style={{
                    fontSize: 15,
                    fontWeight: "600",
                    color: "#1A9E75",
                  }}
                >
                  {item.amount}
                </Text>

                {/* Expand/Collapse Icon */}
                {item.details && (
                  <Ionicons
                    name={isExpanded ? "chevron-up" : "chevron-down"}
                    size={18}
                    color="#393939"
                    style={{ marginLeft: 10 }}
                  />
                )}
              </TouchableOpacity>

              {/* Details Section */}
              {isExpanded && item.details && (
                <View
                  style={{
                    marginTop: 8,
                    backgroundColor: "#F0FFF7",
                    padding: 8,
                    borderRadius: 8,
                  }}
                >
                  <Text style={{ fontSize: 12, color: "#666" }}>
                    {item.details}
                  </Text>
                </View>
              )}
            </View>
          );
        }}
      />
    </View>
  );
};

export default TrafficFineList;








































//First Page code-Monday Code 12.oo clock next add the check navigation option

import React, { useState,useCallback } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView, Modal, FlatList,Image,StyleSheet,Share,StatusBar,LayoutAnimation   } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { List } from "react-native-paper";
import { LinearGradient } from 'expo-linear-gradient';
import * as Font from "expo-font";
import Collapsible from "react-native-collapsible";
import Icon from 'react-native-vector-icons/MaterialIcons';
import { NavigationIndependentTree } from '@react-navigation/native';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
import RNPickerSelect from "react-native-picker-select";
import { MaterialCommunityIcons } from "@expo/vector-icons";
// import Svg, { Rect, Circle } from "react-native-svg";
import Svg, { Rect, Circle, Text as SvgText,Path,Line } from "react-native-svg";
import MaterialIcons from "react-native-vector-icons/MaterialIcons";


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
  const [expanded, setExpanded] = useState(null);
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

  const toggleExpand = (index) => {
    setExpanded(expanded === index ? null : index);
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
      setErrorMessage(""); // ✅ Remove error when valid
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
  <TouchableOpacity onPress={() => navigation.goBack()} style={{ position: "absolute", left: 17 }}>
    <Ionicons name="arrow-back" size={24} color="white" />
  </TouchableOpacity>
  <Text 
    style={{ 
      fontSize: 16, 
      fontWeight: "700", 
      color: "#FFFBFB", 
      fontFamily: 'Poppins',
      marginLeft: 56, // Increased left margin to push text right
    }}
  >
    Pay Challan
  </Text>
</View>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 15, marginTop: 17, elevation: 2, marginHorizontal: 20,paddingBottom: 20,paddingTop: 11 }}>
        <Text style={{ fontSize: 16, fontWeight: "medium", marginBottom: 5,fontFamily: 'Poppins' }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8,fontWeight: "Normal",lineHeight:20  }}>Enter your vehicle number and check {"\n"}your challans</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5, borderRadius: 8 }}>
    <TextInput
      placeholder="Enter Your Vehicle Number"
      value={vehicleNumber}
      onChangeText={validateInput}
      maxLength={10}
      placeholderTextColor="#1A9E75"
      style={{
        width: 190, // Set fixed width instead of flex
        height: 40,
        backgroundColor: "#E6F9F0",
        borderWidth: 1.5,
        borderColor: isValid ? "#1A9E75" : "red",
        borderColor: "#1A9E75",
        borderRadius: 8,
        paddingHorizontal: 8,
        color: "#1A9E75",
        fontWeight: "bold",
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
      <Text style={{ color: "white", fontWeight: "bold" }}>Check</Text>
    </TouchableOpacity>
  </View>
   {/* Error Message (Only show if there's an error) */}
   {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5,alignSelf: "flex-start" }}>
            {errorMessage}
          </Text>
        )}
</View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Normal", marginBottom: 8,marginTop: -5 }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        {/* Step 1 */}
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
    {/* Outer Rounded Rectangle (Main Plate) */}
    <Rect x="10" y="20" width="160" height="60" rx="10" fill="#1A9E75" />

    {/* Inner White Rectangle (Text Background) */}
    <Rect x="20" y="30" width="140" height="40" rx="6" fill="white" />

    {/* Small Circles (Decorative Bolts) */}
    <Circle cx="28" cy="36" r="4" fill="#1A9E75" />
    <Circle cx="152" cy="36" r="4" fill="#1A9E75" />
    <Circle cx="28" cy="62" r="4" fill="#1A9E75" />
    <Circle cx="152" cy="62" r="4" fill="#1A9E75" />

    {/* Text (Number Plate Content) */}
    <SvgText x="90" y="55" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
    








            {/* <MaterialCommunityIcons name="license-plate" size={40} color="#1A9E75" /> */}
            {/* <Ionicons name="car-outline" size={30} color="#1A9E75" /> */}
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Enter your {"\n"} vehicle number</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        {/* Step 2 */}
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
    {/* Document Shape with Right-Bottom Cut */}
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />

    {/* Folded Corner */}
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    {/* Text Lines */}
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    {/* Challan Label */}
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
            {/* <Ionicons name="document-text-outline" size={30} color="#1A9E75" /> */}
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Check Your {"\n"} Challans</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        {/* Step 3 */}
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
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 3</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14 }}>Make {"\n"} Payment</Text>
        </View>
      </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, marginTop: 3, elevation: 2,width: "90%",marginHorizontal: 20 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8 }}>Traffic Fines</Text>{/* ,color:"#393939"*/}
        <Text style={{fontSize: 14,
                      color: "#AFAFAF",
                      marginBottom: 8,marginTop: -3}}>Select your state to see traffic fines</Text>
        
      

      {/* drop down state */}
      

  
      
      {/* Button to Open Modal */}
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
                        marginTop: 25, // 🔥 Decreased top space
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
      fontFamily: "Arial",
      
    }}>
      Invite your friends to make hassle-free
    </Text>
  </View>

      {/* Main Content - Text and Image */}
      <View style={{flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",}}>
        {/* Left Section - Text Content */}
        <View style={{flex: 1,
                      paddingRight: 10,}}>
          <Text style={{color: "white",
    fontSize: 18,
    fontFamily:"Poppins",
    fontWeight: "bold",
    marginBottom: 5,marginLeft:15}}>Challan Payments</Text>
          <Text style={{color: "white",
    fontSize: 10,
    marginBottom: 10,
    marginLeft:15}}>Pay using UPI, Debit & Credit Cards</Text>

          {/* Invite Now Button */}
          <TouchableOpacity style={{backgroundColor: "white",
                                    paddingVertical: 6,
                                    paddingHorizontal: 10,
                                    borderRadius: 15,
                                    alignItems: "center",
                                    alignSelf: "flex-start",marginLeft:15}} 
                                    onPress={shareReferral}>
          <Text style={{alignItems: "flex-end",fontSize:10,fontWeight:"bold",fontFamily:"Poppins"}}>Invite Now</Text>
        </TouchableOpacity>
        </View>

        {/* Right Section - Image */}
        <View style={{width: 80,
                      height: 100,
                      alignItems: "center",
                      justifyContent: "center",
                      borderBottomRightRadius: 1, 
                      paddingTop: 28, 
                      
                       }}>
          <Image
            source={require("../../assets/images/mega1.jpg")}  //mega1.jpg
            style={{width: 120,
              height: 90,
              resizeMode: "contain",}}
          />
        </View>
      </View>
      
    </LinearGradient>
       
       <View style={{marginTop: 15,marginHorizontal:20 }}>
        <Text style={{fontSize: 18, fontWeight: "bold", color: "#333", marginBottom: 10}}>FAQs</Text>
        {faqs.map((faq, index) => (
          <View key={index} style={{backgroundColor: "#FFFFFF", borderRadius: 10, marginBottom: 10, padding: 10, elevation: 2}}>
            <TouchableOpacity style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center", paddingVertical: 10 }} onPress={() => toggleFAQ(index)}>
              <Text style={{fontSize: 15, fontWeight: "600", color: "#333", fontFamily: "Calibri",lineHeight:20}}>{faq.question}</Text>
              <Ionicons name={faq.collapsed ? "chevron-down" : "chevron-up"} size={20} color="#333" />
            </TouchableOpacity>
            <Collapsible collapsed={faq.collapsed}>
              <Text style={{fontSize: 16, color: "#A0A0A0", paddingTop: 4, paddingHorizontal: 5,lineHeight: 18,fontFamily: "Poppins-Regular"}}>{faq.answer}</Text>
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
  const [expandedId, setExpandedId] = useState(null);
  const finesData = [
    { id: "1", title: "Driving Without a \nSeatbelt", amount: "₹ 1000" },
    { id: "2", title: "Carrying Excess \nLuggage", amount: "₹ 500" },
    { id: "3", title: "Triple Riding on \nTwo-vehicle", amount: "₹ 1000" },
    { id: "4", title: "Driving Without a \nNumber Plate", amount: "₹ 500" },
    { id: "5", title: "Driving Without \nHelmet", amount: "₹ 1000" },
    { id: "6", title: "Minor Driving \nVehicle", amount: "₹ 25000" },
    { id: "7", title: "Parking in \nNo-parking Zone", amount: "₹ 500" },
    { id: "8", title: "Dangerous/Rash \nDriving", amount: "₹ 5000" },
    { id: "9", title: "Disobey of Traffic \nSignals", amount: "₹ 5000" },
    { id: "10", title: "Using a Mobile \nPhone While Driving", amount: "₹ 5000" },
    { id: "11", title: "Driving Uninsured Vehicle", amount: "₹ 2000" },
    { id: "12", title: "Drunk Driving", amount: "₹ 10000" },
    { id: "13", title: "Driving Vehicle \nWithout Registration", amount: "₹ 2000" },
    { id: "14", title: "Over-speeding", amount: "₹ 1000" },
    { id: "15", title: "Carrying Explosive/\nInflammable Substances", amount: "₹ 10000" },
    { id: "16", title: "Violation of Road \nRegulations", amount: "₹ 1000" },
    { id: "17", title: "Driving When Mentally \nor Physically Unfit to Drive", amount: "₹ 1000" },
    { id: "18", title: "Not Giving Passage \nto Emergency Vehicles", amount: "₹ 10000" },
    { id: "19", title: "Disqualified Person \nDriving a Vehicle", amount: "₹ 10000" },
    { id: "20", title: "Driving Without \nInsurance", amount: "₹ 2000" },
    { id: "21", title: "Overloading", amount: "₹ 2000" },
    { id: "22", title: "Racing", amount: "₹ 5000" },
    { id: "23", title: "Driving Without a \nValid Driving Licence", amount: "₹ 5000" },
    // { id: "24", title: "Driving a Vehicle \nRegistered in Another \nState for More Than 12 Months", amount: "₹ 500" },
    { id: "24", title: "Failure to Intimate \nChange of the Address \nof Vehicle Owner", amount: "₹ 500" },
  ];

  
  const toggleExpand = (id) => {
    setExpandedId(expandedId === id ? null : id);
  };

  return (
    <View style={{ flex: 1, backgroundColor: "#FFFFFF", }}>
      {/* Header */}
      <View style={{ backgroundColor: "#1A9E75", height: 88, flexDirection: "row", alignItems: "center", paddingHorizontal: 13, paddingTop: 25 }}>
        <TouchableOpacity onPress={() => navigation.goBack()}>
          <Ionicons name="arrow-back" size={24} color="white" />
        </TouchableOpacity>
        <Text style={{ fontSize: 16, fontWeight: "700", color: "white", marginLeft: 12 }}>
          Traffic Fines
        </Text>
      </View>

    {/* Select your State in Fines */}


    <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 10,
          
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#EEEEEE",
          borderWidth:1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          width: "91%",
          marginLeft:20,
          marginTop:20,
        }}
      >
        <Text style={{ color:"#AFAFAF", fontSize: 14,marginLeft:10 }}>
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
          backgroundColor: "#F0FFFA", // Light greenish background
          padding: 9, // Increased padding for better spacing
          borderRadius: 12,
          width: "91%", // Increased width to make it bigger
          alignSelf: "center", // Centers the container
          marginTop: 10, 
        }}>
  <MaterialCommunityIcons name="traffic-light" size={44} color="#1A9E75" paddingHorizontal="5" />
  
  {/* Text Container */}
  <View style={{ flex: 1, marginLeft: 1 }}>  
    <Text style={{
      fontSize: 16, // Slightly larger font
      fontWeight: "bold",
      color: "#333",
      fontFamily: "Poppins_600SemiBold",
      
    }}>
      Traffic fines in Maharashtra 
    </Text>
    
    <Text style={{
      fontSize: 8, // Slightly larger for better readability
      color: "#666",
      marginTop: 5,
      fontFamily: "Poppins_400Regular",
    }}>
      List of traffic fines as per Motor Vehicle (Amendment) Act
    </Text>
  </View>
</View>


<View
  style={{
    padding: 10,
    flex: 1,
    backgroundColor: "#FFFFFF",
    minHeight: "90%",
    marginTop: 4, // Reduced top space
    flexDirection: "row",
    alignItems: "center",
  }}
>
  <FlatList
    data={finesData}
    keyExtractor={(item) => item.id}
    extraData={expandedId}
    contentContainerStyle={{ paddingBottom: 180 }}
    renderItem={({ item }) => {
      const isExpanded = expandedId === item.id;
      return (
          
        <TouchableOpacity onPress={() => toggleExpand(item.id)}
        style={{
          backgroundColor: "#FFFFFF",
          padding: 10,
          borderRadius: 15,
          marginBottom: 13,
          elevation: 3,
          flexDirection: "row",
          alignItems: "center",
          width: "96%",
          alignSelf: "center",
          minHeight: 63,
          paddingHorizontal: 30,
          borderBottomWidth: 1, // Add bottom border
          borderLeftWidth: 1,   // Add left border
          borderRightWidth: 1,  // Add right border
          borderTopWidth: 1,    // Hide top border
          borderColor: "#E4E4E4", // Border color
          
        }}
      >
        <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                <Text style={{ fontSize: 14, fontWeight: "500", color: "#393939", flex: 1 }}>{item.title}</Text>

                <View style={{ flexDirection: "row", alignItems: "center" }}>
                <Text style={{ fontSize: 14, fontWeight: "600", color: "#1A9E75", marginRight: 10 }}>
                  {item.amount}
                </Text>
                <Ionicons name={isExpanded ? "chevron-up" : "chevron-down"} size={18} color="#393939" />
                </View>
        </View>
        {/* Expandable Details Section (Appears Below in New Line) */}
        {isExpanded && (
                <View
                  style={{
                    backgroundColor: "#F0FFFA",
                    padding: 10,
                    borderRadius: 8,
                    marginTop: 10,
                    borderTopWidth: 1,
                    borderTopColor: "#E4E4E4",
                  }}
                >
                  <Text style={{ fontSize: 13, color: "#666" }}>{item.details}</Text>
                </View>
              )}
      </TouchableOpacity>

      )
  }}
  />

  </View>
  </View>

);
};

  const ChallanDetailsScreen = ({ navigation }) => {
    const [activeTab, setActiveTab] = useState("Pending");
  
    return (
      <View style={{ flex: 1, backgroundColor: "#fff" }}>
        {/* Header */}
        <View style={{ backgroundColor: "#1A9E75", height: 80, flexDirection: "row", alignItems: "center", paddingHorizontal: 16, paddingTop: 30 }}>
          <TouchableOpacity onPress={() => navigation.goBack()}>
            <Ionicons name="arrow-back" size={24} color="white" />
          </TouchableOpacity>
          <Text style={{ fontSize: 18, fontWeight: "700", color: "white", marginLeft: 16 }}>Challan Details</Text>
        </View>
  
        {/* Tabs */}
        <View style={{ flexDirection: "row", justifyContent: "space-around", paddingHorizontal:18,marginTop: 10,borderBottomWidth: 1, borderColor: "#ddd",marginLeft:10,marginRight:10 }}>
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Pending" ? "#1A9E75" : "#666" }}>Pending</Text>
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
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Paid" ? "#1A9E75" : "#666" }}>Paid</Text>
          </TouchableOpacity>
          
        </View>
  
        {/* Challan List */}
        {activeTab === "Pending" ? (
          <FlatList
            data={challans}
            keyExtractor={(item) => item.id}
            renderItem={({ item }) => (
              <View style={{backgroundColor: "white", margin: 10, padding: 15, borderRadius: 15, elevation: 2}}>
      
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#000"}}>{item.vehicleNumber}</Text>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#1A9E75"}}>
                {item.amount} 
                </Text>
              </View>

              {/* Due Date */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                {/* Due Date */}
                <Text style={{ fontSize: 14, color: "#888", flex: 1 }}>
                  Due Date: {item.dueDate}
                </Text>
        
                <LinearGradient 
                colors={["#E44E2D", "#E4852E"]} // Red-Orange Gradient
                start={{ x: 0, y: 0 }} 
                end={{ x: 1, y: 0 }} 
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
              <View style={{height: 1, backgroundColor: "#ddd", marginVertical: 8 }} />

              {/* Challan ID & Buttons */}
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <View style={{backgroundColor: "#F5F5F5", paddingVertical: 8, paddingHorizontal: 10, borderRadius: 10}}>
                  <Text style={{fontSize: 11, color: "#393939"}}>{item.challanId}</Text>
                </View>

                <View style={{flexDirection: "row", alignItems: "center"}}>
                  <TouchableOpacity style={{flexDirection: "row", alignItems: "center", marginRight: 10}}>
                    <Text style={{color: "#1A9E75", fontSize: 12}}>More Info</Text>
                    <Ionicons name="chevron-down" size={14} color="#1A9E75" />
                  </TouchableOpacity>

                  <TouchableOpacity style={{backgroundColor: "#1A9E75", paddingVertical: 5, paddingHorizontal: 11, borderRadius: 8}}>
                    <Text style={{color: "#F0FFFA", fontFamily:"Medium", fontSize: 10}}>Pay Now</Text>
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
    width: "90%", // Responsive width
    marginTop: 15,
    alignSelf: "center",
    marginBottom: 5,
    elevation: 5, // Android shadow
    shadowColor: "#000", // iOS shadow
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.2,
    shadowRadius: 4,
    backgroundColor: "#0BC189",
  },
  fullWidthTextContainer: {
    width: "100%", // Ensures full width
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
    paddingTop: 40, // Adjust top padding if needed
  },
  image: {
    width: 70,
    height: 90,
    resizeMode: "contain",
  },

});















































//First Page code-Monday Code 12.oo clock next add the check navigation option

import React, { useState,useCallback } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView, Modal, FlatList,Image,StyleSheet,Share,StatusBar,LayoutAnimation   } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { List } from "react-native-paper";
import { LinearGradient } from 'expo-linear-gradient';
import * as Font from "expo-font";
import Collapsible from "react-native-collapsible";
import Icon from 'react-native-vector-icons/MaterialIcons';
import { NavigationIndependentTree } from '@react-navigation/native';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
import RNPickerSelect from "react-native-picker-select";
import { MaterialCommunityIcons } from "@expo/vector-icons";
// import Svg, { Rect, Circle } from "react-native-svg";
import Svg, { Rect, Circle, Text as SvgText,Path,Line } from "react-native-svg";
import MaterialIcons from "react-native-vector-icons/MaterialIcons";


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
  const [expanded, setExpanded] = useState(null);
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

  const toggleExpand = (index) => {
    setExpanded(expanded === index ? null : index);
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
      setErrorMessage(""); // ✅ Remove error when valid
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
  <TouchableOpacity onPress={() => navigation.goBack()} style={{ position: "absolute", left: 17 }}>
    <Ionicons name="arrow-back" size={24} color="white" />
  </TouchableOpacity>
  <Text 
    style={{ 
      fontSize: 16, 
      fontWeight: "700", 
      color: "#FFFBFB", 
      fontFamily: 'Poppins',
      marginLeft: 56, // Increased left margin to push text right
    }}
  >
    Pay Challan
  </Text>
</View>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 15, marginTop: 17, elevation: 2, marginHorizontal: 20,paddingBottom: 20,paddingTop: 11 }}>
        <Text style={{ fontSize: 16, fontWeight: "medium", marginBottom: 5,fontFamily: 'Poppins' }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8,fontWeight: "Normal",lineHeight:20  }}>Enter your vehicle number and check {"\n"}your challans</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5, borderRadius: 8 }}>
    <TextInput
      placeholder="Enter Your Vehicle Number"
      value={vehicleNumber}
      onChangeText={validateInput}
      maxLength={10}
      placeholderTextColor="#1A9E75"
      style={{
        width: 190, // Set fixed width instead of flex
        height: 40,
        backgroundColor: "#E6F9F0",
        borderWidth: 1.5,
        borderColor: isValid ? "#1A9E75" : "red",
        borderColor: "#1A9E75",
        borderRadius: 8,
        paddingHorizontal: 8,
        color: "#1A9E75",
        fontWeight: "bold",
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
      <Text style={{ color: "white", fontWeight: "bold" }}>Check</Text>
    </TouchableOpacity>
  </View>
   {/* Error Message (Only show if there's an error) */}
   {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5,alignSelf: "flex-start" }}>
            {errorMessage}
          </Text>
        )}
</View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Normal", marginBottom: 8,marginTop: -5 }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        {/* Step 1 */}
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
    {/* Outer Rounded Rectangle (Main Plate) */}
    <Rect x="10" y="20" width="160" height="60" rx="10" fill="#1A9E75" />

    {/* Inner White Rectangle (Text Background) */}
    <Rect x="20" y="30" width="140" height="40" rx="6" fill="white" />

    {/* Small Circles (Decorative Bolts) */}
    <Circle cx="28" cy="36" r="4" fill="#1A9E75" />
    <Circle cx="152" cy="36" r="4" fill="#1A9E75" />
    <Circle cx="28" cy="62" r="4" fill="#1A9E75" />
    <Circle cx="152" cy="62" r="4" fill="#1A9E75" />

    {/* Text (Number Plate Content) */}
    <SvgText x="90" y="55" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
    








            {/* <MaterialCommunityIcons name="license-plate" size={40} color="#1A9E75" /> */}
            {/* <Ionicons name="car-outline" size={30} color="#1A9E75" /> */}
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Enter your {"\n"} vehicle number</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        {/* Step 2 */}
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
    {/* Document Shape with Right-Bottom Cut */}
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />

    {/* Folded Corner */}
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    {/* Text Lines */}
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    {/* Challan Label */}
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
            {/* <Ionicons name="document-text-outline" size={30} color="#1A9E75" /> */}
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Check Your {"\n"} Challans</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        {/* Step 3 */}
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
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 3</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14 }}>Make {"\n"} Payment</Text>
        </View>
      </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, marginTop: 3, elevation: 2,width: "90%",marginHorizontal: 20 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8 }}>Traffic Fines</Text>{/* ,color:"#393939"*/}
        <Text style={{fontSize: 14,
                      color: "#AFAFAF",
                      marginBottom: 8,marginTop: -3}}>Select your state to see traffic fines</Text>
        
      

      {/* drop down state */}
      

  
      
      {/* Button to Open Modal */}
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
                        marginTop: 25, // 🔥 Decreased top space
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
      fontFamily: "Arial",
      
    }}>
      Invite your friends to make hassle-free
    </Text>
  </View>

      {/* Main Content - Text and Image */}
      <View style={{flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",}}>
        {/* Left Section - Text Content */}
        <View style={{flex: 1,
                      paddingRight: 10,}}>
          <Text style={{color: "white",
    fontSize: 18,
    fontFamily:"Poppins",
    fontWeight: "bold",
    marginBottom: 5,marginLeft:15}}>Challan Payments</Text>
          <Text style={{color: "white",
    fontSize: 10,
    marginBottom: 10,
    marginLeft:15}}>Pay using UPI, Debit & Credit Cards</Text>

          {/* Invite Now Button */}
          <TouchableOpacity style={{backgroundColor: "white",
                                    paddingVertical: 6,
                                    paddingHorizontal: 10,
                                    borderRadius: 15,
                                    alignItems: "center",
                                    alignSelf: "flex-start",marginLeft:15}} 
                                    onPress={shareReferral}>
          <Text style={{alignItems: "flex-end",fontSize:10,fontWeight:"bold",fontFamily:"Poppins"}}>Invite Now</Text>
        </TouchableOpacity>
        </View>

        {/* Right Section - Image */}
        <View style={{width: 80,
                      height: 100,
                      alignItems: "center",
                      justifyContent: "center",
                      borderBottomRightRadius: 1, 
                      paddingTop: 28, 
                      
                       }}>
          <Image
            source={require("../../assets/images/mega1.jpg")}  //mega1.jpg
            style={{width: 120,
              height: 90,
              resizeMode: "contain",}}
          />
        </View>
      </View>
      
    </LinearGradient>
       
       <View style={{marginTop: 15,marginHorizontal:20 }}>
        <Text style={{fontSize: 18, fontWeight: "bold", color: "#333", marginBottom: 10}}>FAQs</Text>
        {faqs.map((faq, index) => (
          <View key={index} style={{backgroundColor: "#FFFFFF", borderRadius: 10, marginBottom: 10, padding: 10, elevation: 2}}>
            <TouchableOpacity style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center", paddingVertical: 10 }} onPress={() => toggleFAQ(index)}>
              <Text style={{fontSize: 15, fontWeight: "600", color: "#333", fontFamily: "Calibri",lineHeight:20}}>{faq.question}</Text>
              <Ionicons name={faq.collapsed ? "chevron-down" : "chevron-up"} size={20} color="#333" />
            </TouchableOpacity>
            <Collapsible collapsed={faq.collapsed}>
              <Text style={{fontSize: 16, color: "#A0A0A0", paddingTop: 4, paddingHorizontal: 5,lineHeight: 18,fontFamily: "Poppins-Regular"}}>{faq.answer}</Text>
            </Collapsible>
            
          </View>
        ))}
      </View>
    </ScrollView>
  );
};
const StateChallanScreen = ({ route, navigation }) => {
  const [expandedId, setExpandedId] = useState(null);
  const [modalVisible, setModalVisible] = useState(false);
  const [selectedState, setSelectedState] = useState("");

  const finesData = [
    { id: "1", title: "Driving Without a Seatbelt", amount: "₹ 1000", details: "You must always wear a seatbelt while driving." },
    { id: "2", title: "Carrying Excess Luggage", amount: "₹ 500", details: "Excess luggage beyond the vehicle’s capacity is not allowed." },
    { id: "3", title: "Triple Riding on Two-Wheeler", amount: "₹ 1000", details: "Only two persons are allowed on a two-wheeler." },
    { id: "4", title: "Driving Without a Number Plate", amount: "₹ 500", details: "Number plates must be properly displayed on both sides of the vehicle." },
  ];

  const toggleExpand = useCallback((id) => {
    LayoutAnimation.configureNext(LayoutAnimation.Presets.easeInEaseOut);
    setExpandedId(prevId => (prevId === id ? null : id));
  }, []);

  return (
    <View style={{ flex: 1, backgroundColor: "#FFFFFF" }}>
      {/* Header */}
      <View style={{ backgroundColor: "#1A9E75", height: 88, flexDirection: "row", alignItems: "center", paddingHorizontal: 13, paddingTop: 25 }}>
        <TouchableOpacity onPress={() => navigation.goBack()}>
          <Ionicons name="arrow-back" size={24} color="white" />
        </TouchableOpacity>
        <Text style={{ fontSize: 16, fontWeight: "700", color: "white", marginLeft: 12 }}>
          Traffic Fines
        </Text>
      </View>

      {/* Select State */}
      <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 10,
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#EEEEEE",
          borderWidth: 1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          width: "91%",
          alignSelf: "center",
          marginTop: 20,
        }}
      >
        <Text style={{ color: "#AFAFAF", fontSize: 14, marginLeft: 10 }}>
          {selectedState || "Select Your State"}
        </Text>
        <Ionicons name="caret-down-sharp" size={20} color="#27A47D" />
      </TouchableOpacity>

      {/* List of Fines */}
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        extraData={expandedId}
        contentContainerStyle={{ paddingBottom: 50 }}
        renderItem={({ item }) => {
          const isExpanded = expandedId === item.id;
          return (
            <View style={{
              backgroundColor: "white",
              marginHorizontal: 15,
              marginBottom: 10,
              borderRadius: 10,
              elevation: 3,
              borderWidth: 1,
              borderColor: "#E4E4E4",
            }}>
              <TouchableOpacity onPress={() => toggleExpand(item.id)}
                style={{
                  flexDirection: "row",
                  alignItems: "center",
                  padding: 15,
                  justifyContent: "space-between"
                }}
              >
                <Text style={{
                  fontSize: 14,
                  color: "#393939",
                  fontWeight: "500",
                  flex: 1,
                  textAlign: "left"
                }}>
                  {item.title}
                </Text>

                <Text style={{ fontSize: 14, fontWeight: "bold", color: "#1A9E75" }}>
                  {item.amount}
                </Text>

                <Ionicons name={isExpanded ? "chevron-up" : "chevron-down"} size={20} color="#393939" />
              </TouchableOpacity>

              {/* Expanded Details Section */}
              {isExpanded && (
                <View style={{
                  backgroundColor: "#F0FFFA",
                  padding: 15,
                  borderRadius: 8,
                  marginTop: 5,
                  borderTopWidth: 1,
                  borderTopColor: "#E4E4E4"
                }}>
                  <Text style={{ fontSize: 14, color: "#333" }}>{item.details}</Text>
                </View>
              )}
            </View>
          );
        }}
      />
    </View>
  );
};


  const ChallanDetailsScreen = ({ navigation }) => {
    const [activeTab, setActiveTab] = useState("Pending");
  
    return (
      <View style={{ flex: 1, backgroundColor: "#fff" }}>
        {/* Header */}
        <View style={{ backgroundColor: "#1A9E75", height: 80, flexDirection: "row", alignItems: "center", paddingHorizontal: 16, paddingTop: 30 }}>
          <TouchableOpacity onPress={() => navigation.goBack()}>
            <Ionicons name="arrow-back" size={24} color="white" />
          </TouchableOpacity>
          <Text style={{ fontSize: 18, fontWeight: "700", color: "white", marginLeft: 16 }}>Challan Details</Text>
        </View>
  
        {/* Tabs */}
        <View style={{ flexDirection: "row", justifyContent: "space-around", paddingHorizontal:18,marginTop: 10,borderBottomWidth: 1, borderColor: "#ddd",marginLeft:10,marginRight:10 }}>
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Pending" ? "#1A9E75" : "#666" }}>Pending</Text>
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
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Paid" ? "#1A9E75" : "#666" }}>Paid</Text>
          </TouchableOpacity>
          
        </View>
  
        {/* Challan List */}
        {activeTab === "Pending" ? (
          <FlatList
            data={challans}
            keyExtractor={(item) => item.id}
            renderItem={({ item }) => (
              <View style={{backgroundColor: "white", margin: 10, padding: 15, borderRadius: 15, elevation: 2}}>
      
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#000"}}>{item.vehicleNumber}</Text>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#1A9E75"}}>
                {item.amount} 
                </Text>
              </View>

              {/* Due Date */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                {/* Due Date */}
                <Text style={{ fontSize: 14, color: "#888", flex: 1 }}>
                  Due Date: {item.dueDate}
                </Text>
        
                <LinearGradient 
                colors={["#E44E2D", "#E4852E"]} // Red-Orange Gradient
                start={{ x: 0, y: 0 }} 
                end={{ x: 1, y: 0 }} 
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
              <View style={{height: 1, backgroundColor: "#ddd", marginVertical: 8 }} />

              {/* Challan ID & Buttons */}
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <View style={{backgroundColor: "#F5F5F5", paddingVertical: 8, paddingHorizontal: 10, borderRadius: 10}}>
                  <Text style={{fontSize: 11, color: "#393939"}}>{item.challanId}</Text>
                </View>

                <View style={{flexDirection: "row", alignItems: "center"}}>
                  <TouchableOpacity style={{flexDirection: "row", alignItems: "center", marginRight: 10}}>
                    <Text style={{color: "#1A9E75", fontSize: 12}}>More Info</Text>
                    <Ionicons name="chevron-down" size={14} color="#1A9E75" />
                  </TouchableOpacity>

                  <TouchableOpacity style={{backgroundColor: "#1A9E75", paddingVertical: 5, paddingHorizontal: 11, borderRadius: 8}}>
                    <Text style={{color: "#F0FFFA", fontFamily:"Medium", fontSize: 10}}>Pay Now</Text>
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
    width: "90%", // Responsive width
    marginTop: 15,
    alignSelf: "center",
    marginBottom: 5,
    elevation: 5, // Android shadow
    shadowColor: "#000", // iOS shadow
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.2,
    shadowRadius: 4,
    backgroundColor: "#0BC189",
  },
  fullWidthTextContainer: {
    width: "100%", // Ensures full width
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
    paddingTop: 40, // Adjust top padding if needed
  },
  image: {
    width: 70,
    height: 90,
    resizeMode: "contain",
  },

});












































import React, { useState } from "react";
import { View, Text, FlatList, TouchableOpacity, StyleSheet, Animated } from "react-native";
import { Ionicons } from "@expo/vector-icons";

const TrafficFinesScreen = () => {
  const [expandedItem, setExpandedItem] = useState(null);

  const finesData = [
    { id: "1", title: "Blocking emergency vehicles", amount: "₹ 10,000", details: "" },
    { id: "2", title: "Overloading of passengers", amount: "₹ 1000", details: "" },
    { id: "3", title: "Overspeeding", amount: "₹ 1000", details: "Rs. 1000 for Light Motor Vehicles (LMV) ,\nRs. 2000 for Medium Sized Vehicles" },
    { id: "4", title: "Riding a two-wheeler with more than two persons", amount: "₹ 1000", details: "Rs. 2000 with license suspension for 3 months" },
    { id: "5", title: "Riding without helmet", amount: "₹ 2000", details: "Rs. 2000 with license suspension for 3 months" },
  ];

  const toggleExpand = (id) => {
    setExpandedItem(expandedItem === id ? null : id);
  };

  return (
    <View style={styles.container}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        renderItem={({ item }) => (
          <View style={styles.card}>
            <TouchableOpacity onPress={() => toggleExpand(item.id)} style={styles.header}>
              <Text style={styles.title}>{item.title}</Text>
              <View style={styles.amountContainer}>
                <Text style={styles.amount}>{item.amount}</Text>
                {item.details !== "" && (
                  <Ionicons name={expandedItem === item.id ? "chevron-up" : "chevron-down"} size={20} color="#1A9E75" />
                )}
              </View>
            </TouchableOpacity>

            {expandedItem === item.id && item.details !== "" && (
              <View style={styles.expandedView}>
                <Text style={styles.expandedText}>{item.details}</Text>
              </View>
            )}
          </View>
        )}
        contentContainerStyle={{ paddingBottom: 50 }}
      />
    </View>
  );
};

// **Styles**
const styles = StyleSheet.create({
  container: { flex: 1, backgroundColor: "#FFFFFF", padding: 10 },
  card: {
    backgroundColor: "#FFFFFF",
    padding: 15,
    borderRadius: 12,
    marginBottom: 10,
    elevation: 3,
    borderColor: "#E4E4E4",
    borderWidth: 1,
    width: "96%",
    alignSelf: "center",
  },
  header: { flexDirection: "row", justifyContent: "space-between", alignItems: "center" },
  title: { fontSize: 14, color: "#393939", fontWeight: "bold", flex: 0.7 },
  amountContainer: { flexDirection: "row", alignItems: "center", flex: 0.3, justifyContent: "flex-end" },
  amount: { fontSize: 14, fontWeight: "600", color: "#1A9E75", marginRight: 8 },
  expandedView: { marginTop: 10, backgroundColor: "#F0FFFA", padding: 10, borderRadius: 8 },
  expandedText: { fontSize: 14, color: "#333" },
});

export default TrafficFinesScreen;
































const StateChallanScreen = ({ route, navigation }) => {
  const { stateName } = route.params;
  const [selectedState, setSelectedState] = useState("");
  const [modalVisible, setModalVisible] = useState(false);
  const [expandedId, setExpandedId] = useState(null);

  const finesData = [
    { id: "1", title: "Driving Without a Seatbelt", amount: "₹ 1000" },
    {
      id: "2",
      title: "Carrying Excess Luggage",
      amount: "₹ 500",
      details: "For first offence: ₹500\nFor repeat offence: ₹1500",
    },
    { id: "3", title: "Triple Riding on Two-wheeler", amount: "₹ 1000" },
    {
      id: "4",
      title: "Driving Without a Number Plate",
      amount: "₹ 500",
      details: "For first offence: ₹500\nFor repeat offence: ₹1500",
    },
    { id: "5", title: "Driving Without Helmet", amount: "₹ 1000" },
  ];

  const toggleExpand = (id) => {
    setExpandedId(expandedId === id ? null : id);
  };

  return (
    <View style={{ flex: 1, backgroundColor: "#FFFFFF" }}>
      {/* Header */}
      <View
        style={{
          backgroundColor: "#1A9E75",
          height: 88,
          flexDirection: "row",
          alignItems: "center",
          paddingHorizontal: 13,
          paddingTop: 25,
        }}
      >
        <TouchableOpacity onPress={() => navigation.goBack()}>
          <Ionicons name="arrow-back" size={24} color="white" />
        </TouchableOpacity>
        <Text
          style={{
            fontSize: 16,
            fontWeight: "700",
            color: "white",
            marginLeft: 12,
          }}
        >
          Traffic Fines
        </Text>
      </View>

      {/* Select your State */}
      <TouchableOpacity
        onPress={() => setModalVisible(true)}
        style={{
          backgroundColor: "white",
          padding: 10,
          borderRadius: 10,
          flexDirection: "row",
          borderColor: "#EEEEEE",
          borderWidth: 1,
          justifyContent: "space-between",
          alignItems: "center",
          elevation: 2,
          width: "91%",
          marginLeft: 20,
          marginTop: 20,
        }}
      >
        <Text style={{ color: "#AFAFAF", fontSize: 14, marginLeft: 10 }}>
          {selectedState || "Select Your State"}
        </Text>
        <Ionicons name="caret-down-sharp" size={20} color="#27A47D" />
      </TouchableOpacity>

      {/* Traffic Light Info Box */}
      <View
        style={{
          flexDirection: "row",
          alignItems: "center",
          backgroundColor: "#F0FFFA",
          padding: 9,
          borderRadius: 12,
          width: "91%",
          alignSelf: "center",
          marginTop: 10,
        }}
      >
        <MaterialCommunityIcons
          name="traffic-light"
          size={44}
          color="#1A9E75"
          style={{ paddingHorizontal: 5 }}
        />
        <View style={{ flex: 1, marginLeft: 10 }}>
          <Text
            style={{
              fontSize: 16,
              fontWeight: "bold",
              color: "#333",
            }}
          >
            Traffic fines in {stateName || "Maharashtra"}
          </Text>
          <Text style={{ fontSize: 12, color: "#666", marginTop: 3 }}>
            List of traffic fines as per Motor Vehicle (Amendment) Act
          </Text>
        </View>
      </View>

      {/* Fines List */}
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        contentContainerStyle={{ paddingBottom: 240 }}
        renderItem={({ item }) => (
          <TouchableOpacity
            onPress={() => toggleExpand(item.id)}
            style={{
              backgroundColor: "#FFFFFF",
              padding: 15,
              borderRadius: 15,
              marginBottom: 13,
              elevation: 3,
              flexDirection: "row",
              alignItems: "center",
              width: "96%",
              alignSelf: "center",
              minHeight: 63,
              paddingHorizontal: 20,
              borderBottomWidth: 1,
              borderLeftWidth: 1,
              borderRightWidth: 1,
              borderColor: "#E4E4E4",
            }}
          >
            {/* Title (Expandable) */}
            <View style={{ flex: 1 }}>
              <Text
                style={{
                  fontSize: 14,
                  color: "#393939",
                  fontWeight: "bold",
                  lineHeight: 21,
                }}
              >
                {item.title}
              </Text>
              {/* Expanded Details */}
              {expandedId === item.id && item.details && (
                <Text
                  style={{
                    fontSize: 12,
                    color: "gray",
                    marginTop: 5,
                  }}
                >
                  {item.details}
                </Text>
              )}
            </View>

            {/* Fine Amount & Dropdown Icon */}
            <View style={{ flexDirection: "row", alignItems: "center" }}>
              <Text
                style={{
                  fontSize: 14,
                  fontWeight: "bold",
                  color: "#1A9E75",
                }}
              >
                {item.amount}
              </Text>
              {item.details && (
                <Ionicons
                  name={expandedId === item.id ? "chevron-up" : "chevron-down"}
                  size={20}
                  color="gray"
                  style={{ marginLeft: 10 }}
                />
              )}
            </View>
          </TouchableOpacity>
        )}
      />
    </View>
  );
};

























import React, { useState } from "react";
import { View, FlatList } from "react-native";
import { List, Card, Text } from "react-native-paper";

const finesData = [
  {
    id: "1",
    title: "Overloading of passengers",
    fine: "₹ 1000",
    details: "",
  },
  {
    id: "2",
    title: "Overspeeding",
    fine: "₹ 1000",
    details: "Rs. 1000 for Light Motor Vehicles (LMV), Rs. 2000 for Medium Sized Vehicles",
  },
];

const TrafficFinesScreen = () => {
  const [expanded, setExpanded] = useState(null);

  return (
    <View style={{ flex: 1, padding: 10, backgroundColor: "#f8f9fa" }}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        renderItem={({ item }) => (
          <Card
            style={{
              marginBottom: 10,
              borderRadius: 12,
              backgroundColor: "#fff",
              elevation: 3,
            }}
            onPress={() => setExpanded(expanded === item.id ? null : item.id)}
          >
            <List.Item
              title={item.title}
              titleStyle={{ fontWeight: "bold", fontSize: 16 }}
              right={() => (
                <View style={{ flexDirection: "row", alignItems: "center" }}>
                  <Text style={{ color: "green", fontWeight: "bold", fontSize: 16 }}>
                    {item.fine}
                  </Text>
                  {item.details !== "" && (
                    <List.Icon icon={expanded === item.id ? "chevron-up" : "chevron-down"} />
                  )}
                </View>
              )}
            />
            {expanded === item.id && item.details !== "" && (
              <Text
                style={{
                  paddingHorizontal: 20,
                  paddingBottom: 15,
                  color: "gray",
                  fontSize: 14,
                }}
              >
                {item.details}
              </Text>
            )}
          </Card>
        )}
      />
    </View>
  );
};

export default TrafficFinesScreen;







































import React from "react";
import { View, Text, FlatList } from "react-native";
import { MaterialIcons } from "@expo/vector-icons"; // For Rupee Icon

const finesData = [
  { id: "1", title: "Violation of general traffic rules", amount: "₹ 500" },
  { id: "2", title: "Violation of road regulations", amount: "₹ 500" },
];

const TrafficFineCard = () => {
  return (
    <View style={{ flex: 1, backgroundColor: "#F8F8F8", padding: 10 }}>
      <FlatList
        data={finesData}
        keyExtractor={(item) => item.id}
        renderItem={({ item }) => (
          <View
            style={{
              backgroundColor: "#FFFFFF",
              padding: 15,
              borderRadius: 15,
              marginBottom: 10,
              flexDirection: "row",
              alignItems: "center",
              justifyContent: "space-between",
              elevation: 2, // Shadow for Android
              shadowColor: "#000", // Shadow for iOS
              shadowOffset: { width: 0, height: 1 },
              shadowOpacity: 0.1,
              shadowRadius: 2,
            }}
          >
            {/* Title */}
            <Text
              style={{
                fontSize: 14,
                color: "#333",
                flex: 1,
                fontWeight: "500",
              }}
            >
              {item.title}
            </Text>

            {/* Vertical Divider */}
            <View
              style={{
                width: 1,
                height: "100%",
                backgroundColor: "#E4E4E4",
                marginHorizontal: 15,
              }}
            />

            {/* Amount */}
            <Text
              style={{
                fontSize: 14,
                fontWeight: "600",
                color: "#1A9E75",
              }}
            >
              {item.amount}
            </Text>
          </View>
        )}
      />
    </View>
  );
};

export default TrafficFineCard;

























import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity } from "react-native";

const CheckChallanScreen = ({ navigation }) => {
  const [vehicleNumber, setVehicleNumber] = useState("");
  const [errorMessage, setErrorMessage] = useState("");

  const validateInput = (text) => {
    setVehicleNumber(text);

    
     if (!/^[A-Za-z][A-Za-z0-9]{9}$/.test(text)) {
      setErrorMessage("Invalid format.");
    } 
    else {
      setErrorMessage(""); // ✅ Remove error when valid
    }
  };

  const isValid = /^[A-Za-z][A-Za-z0-9]{9}$/.test(vehicleNumber);

  return (
    <View style={{ backgroundColor: "white", padding: 20, borderRadius: 15, marginTop: 17, elevation: 2, marginHorizontal: 20, paddingBottom: 20 }}>
      
      {/* Heading */}
      <Text style={{ fontSize: 16, fontWeight: "bold", marginBottom: 5, fontFamily: "Poppins" }}>
        Check Challan
      </Text>

      {/* Description */}
      <Text style={{ fontSize: 14, color: "#666", marginBottom: 8, lineHeight: 20 }}>
        Enter your vehicle number and check {"\n"}your challans
      </Text>

      {/* Input Box */}
      <View style={{ marginTop: 5 }}>
        <TextInput
          placeholder="Enter Your Vehicle Number"
          value={vehicleNumber}
          onChangeText={validateInput}
          maxLength={10}
          placeholderTextColor="#1A9E75"
          style={{
            height: 45,
            backgroundColor: "#E6F9F0",
            borderWidth: 1.5,
            borderColor: isValid ? "#1A9E75" : "red",
            borderRadius: 10,
            paddingHorizontal: 12,
            color: "#1A9E75",
            fontWeight: "bold",
            fontSize: 16,
          }}
        />

        {/* Error Message (Only show if there's an error) */}
        {errorMessage !== "" && (
          <Text style={{ color: "red", fontSize: 12, marginTop: 5 }}>
            {errorMessage}
          </Text>
        )}

        {/* Check Button */}
        <TouchableOpacity
          style={{
            backgroundColor: isValid ? "#1A9E75" : "#D3D3D3",
            paddingVertical: 12,
            borderRadius: 10,
            alignItems: "center",
            marginTop: 12,
          }}
          onPress={() => navigation.navigate("ChallanDetailsScreen", { vehicleNumber })}
          disabled={!isValid}
        >
          <Text style={{ color: "white", fontWeight: "bold", fontSize: 16 }}>
            Check
          </Text>
        </TouchableOpacity>

      </View>
    </View>
  );
};

export default CheckChallanScreen;


































import React, { useState } from "react";
import { View, Text, TouchableOpacity } from "react-native";

const Tabs = () => {
  const [activeTab, setActiveTab] = useState("Paid");

  return (
    <View style={{ borderBottomWidth: 1, borderColor: "#ddd",marginLeft:40 }}>
      {/* Tabs Row */}
      <View style={{ flexDirection: "row", justifyContent: "space-around", paddingVertical: 10 }}>
        <TouchableOpacity onPress={() => setActiveTab("Pending")}>
          <Text style={{ 
            fontSize: 18, 
            fontWeight: activeTab === "Pending" ? "bold" : "400", 
            color: "#333" 
          }}>Pending</Text>
        </TouchableOpacity>

        {/* Divider */}
        <View style={{ height: 20, width: 1, backgroundColor: "#ddd" }} />

        <TouchableOpacity onPress={() => setActiveTab("Paid")}>
          <Text style={{ 
            fontSize: 18, 
            fontWeight: activeTab === "Paid" ? "bold" : "400", 
            color: "#333" 
          }}>Paid</Text>
        </TouchableOpacity>
      </View>

      {/* Active Tab Indicator */}
      <View style={{ flexDirection: "row", justifyContent: activeTab === "Paid" ? "flex-end" : "flex-start" }}>
        <View style={{
          width: "40%",
          height: 4,
          backgroundColor: "#008c5f",
          borderRadius: 5,
          marginTop: -3
        }} />
      </View>
    </View>
  );
};

export default Tabs;
































import React, { useState } from "react";
import { View, Text, Image, TouchableOpacity, StyleSheet } from "react-native";

const ChallanScreen = () => {
  const [selectedTab, setSelectedTab] = useState("Paid");

  return (
    <View style={styles.container}>
      {/* Top Tabs */}
      <View style={styles.tabContainer}>
        <TouchableOpacity onPress={() => setSelectedTab("Pending")} style={styles.tab}>
          <Text style={[styles.tabText, selectedTab === "Pending" && styles.activeTabText]}>
            Pending
          </Text>
        </TouchableOpacity>
        <TouchableOpacity onPress={() => setSelectedTab("Paid")} style={styles.tab}>
          <Text style={[styles.tabText, selectedTab === "Paid" && styles.activeTabText]}>
            Paid
          </Text>
          {selectedTab === "Paid" && <View style={styles.underline} />}
        </TouchableOpacity>
      </View>

      {/* Content */}
      <View style={styles.contentContainer}>
        <Image source={require("../../assets/images/challan.jpg")} style={styles.image} />
        <Text style={styles.text}>Checking for challan...</Text>
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: "#fff",
  },
  tabContainer: {
    flexDirection: "row",
    justifyContent: "center",
    borderBottomWidth: 1,
    borderBottomColor: "#ddd",
    paddingVertical: 10,
  },
  tab: {
    marginHorizontal: 20,
  },
  tabText: {
    fontSize: 16,
    color: "#999",
    fontWeight: "600",
  },
  activeTabText: {
    color: "#007b55",
  },
  underline: {
    height: 2,
    backgroundColor: "#007b55",
    width: "100%",
    marginTop: 2,
  },
  contentContainer: {
    flex: 1,
    justifyContent: "center",
    alignItems: "center",
  },
  image: {
    width: 150,
    height: 150,
    resizeMode: "contain",
  },
  text: {
    marginTop: 10,
    fontSize: 14,
    color: "#444",
  },
});

export default ChallanScreen;























import React from "react";
import { View, Text, StyleSheet } from "react-native";
import { LinearGradient } from 'expo-linear-gradient';

const UnpaidLabel = () => {
  return (
    <LinearGradient 
      colors={["#E44D26", "#F16529"]} // Red-Orange Gradient
      start={{ x: 0, y: 0 }} 
      end={{ x: 1, y: 1 }} 
      style={styles.label}
    >
      <Text style={styles.text}>Unpaid</Text>
    </LinearGradient>
  );
};

const styles = StyleSheet.create({
  label: {
    marginTop:50,
    paddingHorizontal: 12,
    paddingVertical: 5,
    borderRadius: 20,
    alignSelf: "flex-start",
  },
  text: {
    color: "#fff",
    fontWeight: "bold",
    fontSize: 14,
  },
});

export default UnpaidLabel;



































//First Page code-Monday Code 12.oo clock next add the check navigation option

import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, ScrollView, Modal, FlatList,Image,StyleSheet,Share,StatusBar   } from "react-native";
import { Ionicons } from "@expo/vector-icons";
import { List } from "react-native-paper";
import { LinearGradient } from 'expo-linear-gradient';
import * as Font from "expo-font";
import Collapsible from "react-native-collapsible";
import Icon from 'react-native-vector-icons/MaterialIcons';
import { NavigationIndependentTree } from '@react-navigation/native';
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
import RNPickerSelect from "react-native-picker-select";
import { MaterialCommunityIcons } from "@expo/vector-icons";
// import Svg, { Rect, Circle } from "react-native-svg";
import Svg, { Rect, Circle, Text as SvgText,Path,Line } from "react-native-svg";


const Stack = createStackNavigator();


const states = [
  "Andaman and Nicobar Islands", "Andhra Pradesh", "Arunachal Pradesh", "Assam",
  "Bihar", "Chandigarh", "Delhi", "Goa", "Gujarat", "Haryana",
  "Jharkhand", "Karnataka", "Kerala", "Madhya Pradesh", "Maharashtra", "Manipur"
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
  const [expanded, setExpanded] = useState(null);
  const [vehicleNumber, setVehicleNumber] = useState("");
  const [modalVisible, setModalVisible] = useState(false);
  const [selectedState, setSelectedState] = useState("");
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

  const toggleExpand = (index) => {
    setExpanded(expanded === index ? null : index);
  };
  const shareReferral = async () => {
    try {
      await Share.share({ message: `Checkout ParkQwik App ${referralCode}` });
    } catch (error) {
      console.log("Error sharing referral code:", error);
    }
  };

  

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
  <TouchableOpacity onPress={() => navigation.goBack()} style={{ position: "absolute", left: 17 }}>
    <Ionicons name="arrow-back" size={24} color="white" />
  </TouchableOpacity>
  <Text 
    style={{ 
      fontSize: 16, 
      fontWeight: "700", 
      color: "#FFFBFB", 
      fontFamily: 'Poppins',
      marginLeft: 56, // Increased left margin to push text right
    }}
  >
    Pay Challan
  </Text>
</View>

      {/* Check Challan Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 15, marginTop: 16, elevation: 2, marginHorizontal: 20,paddingBottom: 22,paddingTop: 8 }}>
        <Text style={{ fontSize: 16, fontWeight: "medium", marginBottom: 5,fontFamily: 'Poppins' }}>Check Challan</Text>
        <Text style={{ fontSize: 14, color: "#666", marginBottom: 8,fontWeight: "Normal",lineHeight:20  }}>Enter your vehicle number and check {"\n"}your challans</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5, borderRadius: 8 }}>
    <TextInput
      placeholder="Enter Your Vehicle Number"
      value={vehicleNumber}
      onChangeText={setVehicleNumber}
      placeholderTextColor="#1A9E75"
      style={{
        width: 190, // Set fixed width instead of flex
        height: 40,
        backgroundColor: "#E6F9F0",
        borderWidth: 1.5,
        borderColor: "#1A9E75",
        borderRadius: 8,
        paddingHorizontal: 8,
        color: "#1A9E75",
        fontWeight: "bold",
      }}
    />
    <TouchableOpacity
      style={{
        backgroundColor: "#D3D3D3",
        paddingVertical: 10,
        paddingHorizontal: 20,
        borderRadius: 8,
        marginLeft: 2,
      }}
      onPress={() => navigation.navigate("ChallanDetailsScreen", { vehicleNumber })}
    >
      <Text style={{ color: "white", fontWeight: "bold" }}>Check</Text>
    </TouchableOpacity>
  </View>
</View>

      {/* How It Works Section */}
      <View style={{ backgroundColor: "white", padding: 20, borderRadius: 10, marginTop: 1, elevatmarginTop: 30,ion: 2 }}>
        <Text style={{ fontSize: 16, fontWeight: "Normal", marginBottom: 8 }}>How it works?</Text>
        <View style={{ flexDirection: "row", alignItems: "center", marginTop: 5 }}>
        {/* Step 1 */}
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
    {/* Outer Rounded Rectangle (Main Plate) */}
    <Rect x="10" y="20" width="160" height="60" rx="10" fill="#1A9E75" />

    {/* Inner White Rectangle (Text Background) */}
    <Rect x="20" y="30" width="140" height="40" rx="6" fill="white" />

    {/* Small Circles (Decorative Bolts) */}
    <Circle cx="28" cy="36" r="4" fill="#1A9E75" />
    <Circle cx="152" cy="36" r="4" fill="#1A9E75" />
    <Circle cx="28" cy="62" r="4" fill="#1A9E75" />
    <Circle cx="152" cy="62" r="4" fill="#1A9E75" />

    {/* Text (Number Plate Content) */}
    <SvgText x="90" y="55" fontSize="22" fontWeight="bold" fill="#1A9E75" textAnchor="middle">
      XYZ-123
    </SvgText>
  </Svg>
            {/* <MaterialCommunityIcons name="license-plate" size={40} color="#1A9E75" /> */}
            {/* <Ionicons name="car-outline" size={30} color="#1A9E75" /> */}
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 5,marginLeft: 15 }}>Step 1</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Enter your {"\n"} vehicle number</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }} />

        {/* Step 2 */}
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
    {/* Document Shape with Right-Bottom Cut */}
    <Path
      d="M12 8 H50 V44 L40 56 H12 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="3"
    />

    {/* Folded Corner */}
    <Path
      d="M50 44 L40 56 V44 H50 Z"
      fill="white"
      stroke="#1A9E75"
      strokeWidth="2"
    />

    {/* Text Lines */}
    <Line x1="16" y1="18" x2="48" y2="18" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="24" x2="48" y2="24" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="30" x2="48" y2="30" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="36" x2="48" y2="36" stroke="#1A9E75" strokeWidth="2" />
    <Line x1="16" y1="42" x2="48" y2="42" stroke="#1A9E75" strokeWidth="2" />

    {/* Challan Label */}
    <SvgText x="18" y="16" fontSize="6" fontWeight="bold" fill="#1A9E75">
      CHALLAN
    </SvgText>
  </Svg>
            {/* <Ionicons name="document-text-outline" size={30} color="#1A9E75" /> */}
          </View>
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 2</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",fontWeight: "Normal",lineHeight:14 }}>Check Your {"\n"} Challans</Text>
        </View>

        {/* Arrow */}
        <Ionicons name="arrow-forward" size={24} color="#1A9E75" style={{ marginTop: -45,marginHorizontal:8 }}/>

        {/* Step 3 */}
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
          <Text style={{ fontSize: 14, fontWeight: "bold", color: "#333", marginTop: 4,marginLeft: 15 }}>Step 3</Text>
          <Text style={{ fontSize: 10, color: "#777", textAlign: "center",lineHeight:14 }}>Make {"\n"} Payment</Text>
        </View>
      </View>
      </View>

      {/* Traffic Fines Section */}
      <View style={{ backgroundColor: "white", padding: 15, borderRadius: 10, marginTop: -1, elevation: 2,width: "90%",marginHorizontal: 20 }}>
        <Text style={{ fontSize: 16, fontWeight: "Medium", marginBottom: 8 }}>Traffic Fines</Text>{/* ,color:"#393939"*/}
        <Text style={{fontSize: 14,
                      color: "#AFAFAF",
                      marginBottom: 10,marginTop: -5}}>Select your state to see traffic fines</Text>
        
      

      {/* drop down state */}
      

  
      
      {/* Button to Open Modal */}
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
        }}
      >
        <Text style={{ color:"#AFAFAF", fontSize: 14 }}>
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
              <Text style={{ fontSize: 18, fontWeight: "bold", color: "#333" }}>Choose State</Text>
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
                    style={{ flexDirection: "row", alignItems: "center", paddingVertical: 10 }}
                    onPress={() => {
                      setModalVisible(false);
                      navigation.navigate("StateChallanScreen", { stateName: item });
                    }}
                  >
                    <Ionicons name="radio-button-off" size={20} color="green" style={{ marginRight: 10 }} />
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
      <LinearGradient colors={["#0BC189", "#27CD99"]} style={styles.container}>
      
      {/* Left Section - Text */}
      <View style={styles.textContainer}>
        <Text style={styles.inviteText}>
          Invite your friends to make hassle-free
        </Text>
        <Text style={styles.title}>Challan Payments</Text>
        <Text style={styles.description}>
          Pay using UPI, Debit & Credit Cards
        </Text>

        <TouchableOpacity style={styles.button} onPress={shareReferral}>
          <Text style={styles.buttonText}>Invite Now</Text>
        </TouchableOpacity>
      </View>

      {/* Right Section - Image */}
      <View style={styles.imageContainer}>
        <Image 
          source={require("../../assets/images/loudspeaker (3).png")} 
          style={styles.image} 
        />
      </View>

    </LinearGradient>
       
       <View style={{marginTop: 15,marginHorizontal:20 }}>
        <Text style={{fontSize: 18, fontWeight: "bold", color: "#333", marginBottom: 10}}>FAQs</Text>
        {faqs.map((faq, index) => (
          <View key={index} style={{backgroundColor: "#FFFFFF", borderRadius: 10, marginBottom: 10, padding: 10, elevation: 2}}>
            <TouchableOpacity style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center", paddingVertical: 10 }} onPress={() => toggleFAQ(index)}>
              <Text style={{fontSize: 15, fontWeight: "600", color: "#333"}}>{faq.question}</Text>
              <Ionicons name={faq.collapsed ? "chevron-down" : "chevron-up"} size={20} color="#333" />
            </TouchableOpacity>
            <Collapsible collapsed={faq.collapsed}>
              <Text style={{fontSize: 16, color: "#A0A0A0", paddingTop: 4, paddingHorizontal: 5,lineHeight: 18,fontFamily: "Poppins-Regular"}}>{faq.answer}</Text>
            </Collapsible>
            
          </View>
        ))}
      </View>
    </ScrollView>
  );
};
const StateChallanScreen = ({ route, navigation }) => {
  const { stateName } = route.params;
  const finesData = [
    { id: "1", title: "Driving Without a \nSeatbelt", amount: "₹ 1000" },
    { id: "2", title: "Carrying Excess \nLuggage", amount: "₹ 500" },
    { id: "3", title: "Triple Riding on \nTwo-vehicle", amount: "₹ 1000" },
    { id: "4", title: "Driving Without a \nNumber Plate", amount: "₹ 500" },
    { id: "5", title: "Driving Without \nHelmet", amount: "₹ 1000" },
    { id: "6", title: "Minor Driving \nVehicle", amount: "₹ 25000" },
    { id: "7", title: "Parking in \nNo-parking Zone", amount: "₹ 500" },
    { id: "8", title: "Dangerous/Rash \nDriving", amount: "₹ 5000" },
    { id: "9", title: "Disobey of Traffic \nSignals", amount: "₹ 5000" },
    { id: "10", title: "Using a Mobile \nPhone While Driving", amount: "₹ 5000" },
    { id: "11", title: "Driving Uninsured Vehicle", amount: "₹ 2000" },
    { id: "12", title: "Drunk Driving", amount: "₹ 10000" },
    { id: "13", title: "Driving Vehicle \nWithout Registration", amount: "₹ 2000" },
    { id: "14", title: "Over-speeding", amount: "₹ 1000" },
    { id: "15", title: "Carrying Explosive/\nInflammable Substances", amount: "₹ 10000" },
    { id: "16", title: "Violation of Road \nRegulations", amount: "₹ 1000" },
    { id: "17", title: "Driving When Mentally \nor Physically Unfit to Drive", amount: "₹ 1000" },
    { id: "18", title: "Not Giving Passage \nto Emergency Vehicles", amount: "₹ 10000" },
    { id: "19", title: "Disqualified Person \nDriving a Vehicle", amount: "₹ 10000" },
    { id: "20", title: "Driving Without \nInsurance", amount: "₹ 2000" },
    { id: "21", title: "Overloading", amount: "₹ 2000" },
    { id: "22", title: "Racing", amount: "₹ 5000" },
    { id: "23", title: "Driving Without a \nValid Driving Licence", amount: "₹ 5000" },
    // { id: "24", title: "Driving a Vehicle \nRegistered in Another \nState for More Than 12 Months", amount: "₹ 500" },
    { id: "24", title: "Failure to Intimate \nChange of the Address \nof Vehicle Owner", amount: "₹ 500" },
  ];

  return (
    <View style={{ flex: 1, backgroundColor: "#FFFFFF" }}>
      {/* Header */}
      <View style={{ backgroundColor: "#1A9E75", height: 88, flexDirection: "row", alignItems: "center", paddingHorizontal: 20, paddingTop: 25 }}>
        <TouchableOpacity onPress={() => navigation.goBack()}>
          <Ionicons name="arrow-back" size={24} color="white" />
        </TouchableOpacity>
        <Text style={{ fontSize: 16, fontWeight: "700", color: "white", marginLeft: 20 }}>
          Traffic Fines
        </Text>
      </View>

      <View style={{  justifyContent: "flex-start", alignItems: "center", backgroundColor: "#FFFFFF", paddingTop: 20 }}>
          <View style={{
            backgroundColor: "#fff",
            borderRadius: 8,
            padding: 8,
            elevation: 2,
            width: "90%",
            position: "relative",
          }}>
            <RNPickerSelect
              onValueChange={(value) => setSelectedState(value)}
              items={[
                { label: "Tamil Nadu", value: "Tamil Nadu" },
                { label: "Maharashtra", value: "Maharashtra" },
                { label: "Karnataka", value: "Karnataka" },
              ]}
              placeholder={{ label: "Select Your State", value: null }}
              style={pickerSelectStyles}
              useNativeAndroidPickerStyle={false}
              Icon={() => (
                <View style={{ right: 15, top: "15%",  }}>
                  <Ionicons name="caret-down-sharp" size={20} color="#27A47D" />
                </View>
              )}
            />
          </View>
        </View>

    {/* Traffic light */}
    <View style={{
          flexDirection: "row",
          alignItems: "center",
          backgroundColor: "#F0FFFA", // Light greenish background
          padding: 9, // Increased padding for better spacing
          borderRadius: 12,
          width: "92%", // Increased width to make it bigger
          alignSelf: "center", // Centers the container
          marginTop: 10, 
        }}>
  <MaterialCommunityIcons name="traffic-light" size={40} color="#1A9E75" paddingHorizontal="15" />
  
  {/* Text Container */}
  <View style={{ flex: 1, marginLeft: 1 }}>  
    <Text style={{
      fontSize: 16, // Slightly larger font
      fontWeight: "bold",
      color: "#333",
      fontFamily: "Poppins_600SemiBold",
    }}>
      Traffic fines in Tamil Nadu
    </Text>
    
    <Text style={{
      fontSize: 8, // Slightly larger for better readability
      color: "#666",
      marginTop: 5,
      fontFamily: "Poppins_400Regular",
    }}>
      List of traffic fines as per Motor Vehicle (Amendment) Act
    </Text>
  </View>
</View>


<View
  style={{
    padding: 10,
    backgroundColor: "#FFFFFF",
    minHeight: "90%",
    marginTop: 4, // Reduced top space
    flexDirection: "row",
    alignItems: "center",
  }}
>
  <FlatList
    data={finesData}
    keyExtractor={(item) => item.id}
    contentContainerStyle={{ paddingBottom: 240 }}
    renderItem={({ item }) => (
      <View
        style={{
          backgroundColor: "#FFFFFF",
          padding: 10,
          borderRadius: 10,
          marginBottom: 13,
          elevation: 3,
          flexDirection: "row",
          alignItems: "center",
          width: "99%",
          alignSelf: "center",
          minHeight: 63,
          paddingHorizontal: 30,
          
        }}
      >
        {/* Title (70% Width) */}
        <Text
          style={{
            fontSize: 14,
            color: "#393939",
            fontFamily: "Poppins_400Regular",
            flex: 0.7,
            textAlign: "left",
            lineHeight:21,
          }}
        >
          {item.title}
        </Text>

        {/* Vertical Separator */}
        <View
          style={{
            height: 27,
            width: 1,
            backgroundColor: "#E4E4E4",
            marginLeft: "5%",
            alignSelf: "center",
          }}
        />

        {/* Amount (30% Width) */}
        <Text
          style={{
            fontSize: 14,
            fontWeight: "600",
            color: "#1A9E75",
            fontFamily: "Poppins_600SemiBold",
            flex: 0.3,
            textAlign: "right",
            paddingRight: 10,
          }}
        >
          {item.amount}
        </Text>
      </View>
    )}
  />
</View>
    </View>
    );
  };


  const ChallanDetailsScreen = ({ navigation }) => {
    const [activeTab, setActiveTab] = useState("Pending");
  
    return (
      <View style={{ flex: 1, backgroundColor: "#fff" }}>
        {/* Header */}
        <View style={{ backgroundColor: "#1A9E75", height: 80, flexDirection: "row", alignItems: "center", paddingHorizontal: 16, paddingTop: 30 }}>
          <TouchableOpacity onPress={() => navigation.goBack()}>
            <Ionicons name="arrow-back" size={24} color="white" />
          </TouchableOpacity>
          <Text style={{ fontSize: 18, fontWeight: "700", color: "white", marginLeft: 16 }}>Challan Details</Text>
        </View>
  
        {/* Tabs */}
        <View style={{ flexDirection: "row", justifyContent: "space-around", borderBottomWidth: 1, borderColor: "#ddd", marginTop: 10 }}>
          <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Pending" ? "#1A9E75" : "#666" }}>Pending</Text>
          </TouchableOpacity>
          <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
            <Text style={{ fontSize: 16, color: activeTab === "Paid" ? "#1A9E75" : "#666" }}>Paid</Text>
          </TouchableOpacity>
        </View>
  
        {/* Challan List */}
        {activeTab === "Pending" ? (
          <FlatList
            data={challans}
            keyExtractor={(item) => item.id}
            renderItem={({ item }) => (
              <View style={{backgroundColor: "white", margin: 10, padding: 15, borderRadius: 12, elevation: 2}}>
      
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#000"}}>{item.vehicleNumber}</Text>
                <Text style={{fontSize: 16, fontWeight: "bold", color: "#1A9E75"}}>
                {item.amount} 
                </Text>
              </View>

              {/* Due Date */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", alignItems: "center" }}>
                {/* Due Date */}
                <Text style={{ fontSize: 14, color: "#888", flex: 1 }}>
                  Due Date: {item.dueDate}
                </Text>

                {/* Status Badge - Moves to the End */}
                <View style={{
                  backgroundColor: "#E74C3C",
                  paddingVertical: 2,
                  paddingHorizontal: 8,
                  borderRadius: 4,
                  alignSelf: "center" // Aligns it properly
                }}>
                  <Text style={{ fontSize: 12, color: "white" }}>{item.status}</Text>
                </View>
              </View>

              {/* Divider */}
              <View style={{height: 1, backgroundColor: "#ddd", marginVertical: 8 }} />

              {/* Challan ID & Buttons */}
              <View style={{flexDirection: "row", justifyContent: "space-between", alignItems: "center"}}>
                <View style={{backgroundColor: "#F5F5F5", paddingVertical: 8, paddingHorizontal: 10, borderRadius: 10}}>
                  <Text style={{fontSize: 11, color: "#393939"}}>{item.challanId}</Text>
                </View>

                <View style={{flexDirection: "row", alignItems: "center"}}>
                  <TouchableOpacity style={{flexDirection: "row", alignItems: "center", marginRight: 10}}>
                    <Text style={{color: "#1A9E75", fontSize: 12}}>More Info</Text>
                    <Ionicons name="chevron-down" size={14} color="#1A9E75" />
                  </TouchableOpacity>

                  <TouchableOpacity style={{backgroundColor: "#1A9E75", paddingVertical: 8, paddingHorizontal: 15, borderRadius: 8}}>
                    <Text style={{color: "#F0FFFA", fontFamily:"Medium", fontSize: 10}}>Pay Now</Text>
                  </TouchableOpacity>
                </View>
              </View>
              </View>
            )}
          />
        ) : (
          <View style={{ flex: 1, justifyContent: "center", alignItems: "center" }}>
            <Text style={{ fontSize: 16, color: "#666" }}>No paid challans found.</Text>
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
  container: {
    padding: 20,
    borderRadius: 15,
    flexDirection: "row",
    alignItems: "center",
    justifyContent: "space-between",
    width: "90%", 
    marginTop: 15,
    alignSelf: "center",
    marginBottom: 5,
    elevation: 5,
    shadowColor: "#000",
    shadowOpacity: 0.1,
    shadowRadius: 4,
    shadowOffset: { width: 0, height: 2 },
  },
  textContainer: {
    flex: 1,
    paddingRight: 10, // Added space between text and image
  },
  inviteText: {
    color: "#FBFF29",
    fontSize: 15,
    fontFamily: "Arial",
    marginBottom: 5,
  },
  title: {
    color: "white",
    fontSize: 20,
    fontFamily: "Arial",
    fontWeight: "bold",
    marginBottom: 5,
  },
  description: {
    color: "white",
    fontSize: 12,
    marginBottom: 10,
  },
  button: {
    backgroundColor: "white",
    paddingVertical: 8,
    paddingHorizontal: 15,
    borderRadius: 12,
    alignItems: "center",
    alignSelf: "flex-start",
  },
  buttonText: {
    color: "#0BC189",
    fontWeight: "bold",
  },
  imageContainer: {
    width: 80,
    height: 100,
    alignItems: "center",
    justifyContent: "center",
    paddingTop:90,
  },
  image: {
    width: 70,
    height: 100, 
    resizeMode: "contain",
  },
});

const pickerSelectStyles = {
  inputIOS: { fontSize: 16, padding: 12, borderRadius: 8, color: "#666" },
  inputAndroid: { fontSize: 16, padding: 1, borderRadius: 8, color: "#666",paddingHorizontal:19 },
};






















/////////////////////////////

/<div> Icons made by <a href="https://www.flaticon.com/authors/freepik" title="Freepik"> Freepik </a> from <a href="https://www.flaticon.com/" title="Flaticon">www.flaticon.com'</a></div>


import React from "react";
import { View, Text, TouchableOpacity, StyleSheet } from "react-native";
import { Ionicons } from "@expo/vector-icons";

const ChallanCard = () => {
  return (
    <View style={styles.card}>
      {/* Top Row */}
      <View style={styles.row}>
        <Text style={styles.vehicleNumber}>TN01B5230</Text>
        <Text style={styles.amount}>
          ₹ 600 <Text style={styles.unpaidBadge}>Unpaid</Text>
        </Text>
      </View>

      {/* Due Date */}
      <Text style={styles.dueDate}>Due Date: 20 Sep 2023</Text>

      {/* Divider */}
      <View style={styles.divider} />

      {/* Challan ID & Buttons */}
      <View style={styles.row}>
        <View style={styles.challanContainer}>
          <Text style={styles.challanText}>TN246972306251330123</Text>
        </View>

        <View style={styles.buttonRow}>
          <TouchableOpacity style={styles.moreInfo}>
            <Text style={styles.moreInfoText}>More Info</Text>
            <Ionicons name="chevron-down" size={14} color="#1A9E75" />
          </TouchableOpacity>

          <TouchableOpacity style={styles.payNow}>
            <Text style={styles.payNowText}>Pay Now</Text>
          </TouchableOpacity>
        </View>
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  card: { backgroundColor: "white", margin: 10, padding: 15, borderRadius: 12, elevation: 2 },
  row: { flexDirection: "row", justifyContent: "space-between", alignItems: "center" },

  vehicleNumber: { fontSize: 16, fontWeight: "bold", color: "#000" },
  amount: { fontSize: 16, fontWeight: "bold", color: "#1A9E75" },
  unpaidBadge: { fontSize: 12, color: "white", backgroundColor: "#E74C3C", paddingVertical: 2, paddingHorizontal: 6, borderRadius: 4, overflow: "hidden" },

  dueDate: { fontSize: 14, color: "#666", marginVertical: 4 },
  divider: { height: 1, backgroundColor: "#ddd", marginVertical: 8 },

  challanContainer: { backgroundColor: "#E6E6E6", paddingVertical: 6, paddingHorizontal: 10, borderRadius: 6 },
  challanText: { fontSize: 12, color: "#666" },

  buttonRow: { flexDirection: "row", alignItems: "center" },
  moreInfo: { flexDirection: "row", alignItems: "center", marginRight: 10 },
  moreInfoText: { color: "#1A9E75", fontSize: 14 },

  payNow: { backgroundColor: "#1A9E75", paddingVertical: 8, paddingHorizontal: 15, borderRadius: 6 },
  payNowText: { color: "white", fontWeight: "bold" },
});

export default ChallanCard;


























<!-- import React, { useState } from "react";
import { View, Text, TouchableOpacity, FlatList } from "react-native";
import { Ionicons } from "@expo/vector-icons";

const challans = [
  {
    id: "1",
    vehicleNumber: "TN01B5230",
    dueDate: "20 Sep 2023",
    amount: "₹ 600",
    status: "Unpaid",
    challanId: "TN246972306251330123",
  },
  {
    id: "2",
    vehicleNumber: "TN01B5230",
    dueDate: "10 Nov 2023",
    amount: "₹ 200",
    status: "Unpaid",
    challanId: "TN246972306251330123",
  },
];

const ChallanDetailsScreen = ({ navigation }) => {
  const [activeTab, setActiveTab] = useState("Pending");

  return (
    <View style={{ flex: 1, backgroundColor: "#fff" }}>
      {/* Header */}
      <View style={{ backgroundColor: "#1A9E75", height: 80, flexDirection: "row", alignItems: "center", paddingHorizontal: 16, paddingTop: 30 }}>
        <TouchableOpacity onPress={() => navigation.goBack()}>
          <Ionicons name="arrow-back" size={24} color="white" />
        </TouchableOpacity>
        <Text style={{ fontSize: 18, fontWeight: "700", color: "white", marginLeft: 16 }}>Challan Details</Text>
      </View>

      {/* Tabs */}
      <View style={{ flexDirection: "row", justifyContent: "space-around", borderBottomWidth: 1, borderColor: "#ddd", marginTop: 10 }}>
        <TouchableOpacity onPress={() => setActiveTab("Pending")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Pending" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
          <Text style={{ fontSize: 16, color: activeTab === "Pending" ? "#1A9E75" : "#666" }}>Pending</Text>
        </TouchableOpacity>
        <TouchableOpacity onPress={() => setActiveTab("Paid")} style={{ paddingVertical: 10, borderBottomWidth: activeTab === "Paid" ? 2 : 0, borderBottomColor: "#1A9E75" }}>
          <Text style={{ fontSize: 16, color: activeTab === "Paid" ? "#1A9E75" : "#666" }}>Paid</Text>
        </TouchableOpacity>
      </View>

      {/* Challan List */}
      {activeTab === "Pending" ? (
        <FlatList
          data={challans}
          keyExtractor={(item) => item.id}
          renderItem={({ item }) => (
            <View style={{ backgroundColor: "white", margin: 10, padding: 15, borderRadius: 8, shadowColor: "#000", shadowOpacity: 0.1, shadowRadius: 4, elevation: 2 }}>
              <Text style={{ fontSize: 16, fontWeight: "bold", color: "#000" }}>{item.vehicleNumber}</Text>
              <Text style={{ fontSize: 14, color: "#666", marginVertical: 4 }}>Due Date: {item.dueDate}</Text>
              <Text style={{ fontSize: 16, fontWeight: "bold", color: "#1A9E75" }}>{item.amount} <Text style={{ fontSize: 12, color: "red" }}>{item.status}</Text></Text>
              <Text style={{ fontSize: 12, color: "#666", marginTop: 5 }}>{item.challanId}</Text>
              
              {/* Buttons */}
              <View style={{ flexDirection: "row", justifyContent: "space-between", marginTop: 10 }}>
                <TouchableOpacity style={{ flexDirection: "row", alignItems: "center" }}>
                  <Text style={{ color: "#1A9E75", fontSize: 14 }}>More Info</Text>
                  <Ionicons name="chevron-down" size={16} color="#1A9E75" />
                </TouchableOpacity>
                <TouchableOpacity style={{ backgroundColor: "#1A9E75", paddingVertical: 8, paddingHorizontal: 12, borderRadius: 5 }}>
                  <Text style={{ color: "white", fontWeight: "bold" }}>Pay Now</Text>
                </TouchableOpacity>
              </View>
            </View>
          )}
        />
      ) : (
        <View style={{ flex: 1, justifyContent: "center", alignItems: "center" }}>
          <Text style={{ fontSize: 16, color: "#666" }}>No paid challans found.</Text>
        </View>
      )}
    </View>
  );
};

export default ChallanDetailsScreen; -->
