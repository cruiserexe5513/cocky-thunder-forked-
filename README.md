import React, { useState, useEffect, useRef } from "react";
// Chart.js'i import etmeye gerek yok, script ile yüklenecek.

// --- Stil Tanımları (Tüm Güncellemeler Dahil) ---
const styles = {
  appContainer: {
    minHeight: "100vh",
    backgroundColor: "#2A2E37",
    color: "#EAEAEA",
    padding: "20px",
    fontFamily: "'Poppins', sans-serif",
  },
  header: {
    maxWidth: "60rem",
    margin: "0 auto",
    textAlign: "center",
    marginBottom: "50px",
  },
  h1: {
    fontSize: "40px",
    fontWeight: "700",
    color: "white",
    marginBottom: "16px",
    letterSpacing: "-0.03em",
    lineHeight: "1.2",
  },
  p: {
    color: "#C0C0C0",
    fontSize: "19px",
    lineHeight: "1.7",
    maxWidth: "50rem",
    margin: "0 auto",
  },
  main: { maxWidth: "50rem", margin: "0 auto" },
  footer: {
    textAlign: "center",
    color: "#888EA8",
    marginTop: "70px",
    fontSize: "15px",
    paddingBottom: "25px",
    lineHeight: "1.6",
  },
  card: {
    backgroundColor: "#353A47",
    borderRadius: "20px",
    boxShadow:
      "0 12px 25px -8px rgba(0, 0, 0, 0.25), 0 6px 12px -6px rgba(0, 0, 0, 0.15)",
    padding: "32px",
    marginBottom: "36px",
    borderTopWidth: "5px",
    borderTopStyle: "solid",
  },
  cardTitle: {
    fontSize: "26px",
    fontWeight: "600",
    marginBottom: "24px",
    color: "white",
    textAlign: "center",
  },
  form: {
    display: "grid",
    gridTemplateColumns: "repeat(2, 1fr)",
    gap: "22px",
    marginBottom: "24px",
  },
  input: {
    padding: "16px",
    border: "1px solid #505662",
    backgroundColor: "#2A2E37",
    color: "#EAEAEA",
    borderRadius: "10px",
    fontSize: "17px",
    WebkitBoxShadow: "0 0 0 30px #2A2E37 inset !important",
    boxShadow: "0 0 0 30px #2A2E37 inset !important",
    WebkitTextFillColor: "#EAEAEA !important",
  },
  inputFull: { gridColumn: "span 2" },
  select: {
    gridColumn: "span 2",
    padding: "16px",
    border: "1px solid #505662",
    backgroundColor: "#2A2E37",
    color: "#EAEAEA",
    borderRadius: "10px",
    fontSize: "17px",
  },
  buttonBase: {
    gridColumn: "span 2",
    marginTop: "16px",
    padding: "18px",
    borderRadius: "10px",
    color: "white",
    fontWeight: "600",
    fontSize: "18px",
    border: "none",
    cursor: "pointer",
    transition:
      "background-color 0.2s ease-in-out, transform 0.15s ease-in-out, box-shadow 0.2s ease",
    textAlign: "center",
  },
  buttonFemale: {
    backgroundColor: "#FF69B4",
    boxShadow: "0 4px 6px rgba(255, 105, 180, 0.25)",
  },
  buttonFemaleHover: {
    backgroundColor: "#FF1493",
    transform: "scale(1.03) translateY(-2px)",
    boxShadow: "0 6px 12px rgba(255, 20, 147, 0.35)",
  },
  buttonMale: {
    backgroundColor: "#1E90FF",
    boxShadow: "0 4px 6px rgba(30, 144, 255, 0.25)",
  },
  buttonMaleHover: {
    backgroundColor: "#007FFF",
    transform: "scale(1.03) translateY(-2px)",
    boxShadow: "0 6px 12px rgba(0, 127, 255, 0.35)",
  },
  resultText: {
    fontSize: "22px",
    fontWeight: "600",
    color: "#66CDAA",
    textAlign: "center",
    margin: "20px 0",
    padding: "10px",
    backgroundColor: "rgba(102, 205, 170, 0.1)",
    borderRadius: "8px",
  },
  nutritionGuidanceSection: {
    marginTop: "28px",
    padding: "24px",
    backgroundColor: "rgba(255, 255, 255, 0.03)",
    borderRadius: "12px",
    border: "1px dashed #505662",
  },
  nutritionTitle: {
    fontSize: "20px",
    fontWeight: "600",
    color: "#FFD700",
    marginBottom: "16px",
    textAlign: "center",
  },
  nutritionText: {
    fontSize: "16px",
    color: "#E0E0E0",
    lineHeight: "1.7",
    marginBottom: "12px",
  },
  nutritionDisclaimer: {
    fontSize: "14px",
    color: "#A0AEC0",
    fontStyle: "italic",
    textAlign: "center",
    marginTop: "18px",
  },
  workoutSection: {
    marginTop: "32px",
    paddingTop: "28px",
    borderTop: "1px solid #505662",
  },
  workoutTitle: {
    fontSize: "22px",
    fontWeight: "600",
    color: "white",
    marginBottom: "18px",
    textAlign: "center",
  },
  workoutSuggestionList: {
    listStyle: "none",
    paddingLeft: "0px",
    marginBottom: "18px",
  },
  workoutSuggestionItem: {
    color: "#EAEAEA",
    marginBottom: "12px",
    lineHeight: "1.7",
    padding: "12px",
    backgroundColor: "rgba(255, 255, 255, 0.03)",
    borderRadius: "8px",
    display: "flex",
    alignItems: "center",
  },
  workoutSuggestionIcon: { marginRight: "12px", fontSize: "18px" },
  workoutNote: {
    fontSize: "15px",
    color: "#B0B0B0",
    fontStyle: "italic",
    lineHeight: "1.6",
    textAlign: "center",
    padding: "10px",
    backgroundColor: "rgba(255, 255, 255, 0.02)",
    borderRadius: "8px",
    marginTop: "10px",
  },
  locationActionSection: {
    marginTop: "32px",
    padding: "28px",
    backgroundColor: "#3C4252",
    borderRadius: "16px",
    textAlign: "center",
    border: "1px solid #505662",
  },
  locationActionTitle: {
    fontSize: "22px",
    fontWeight: "600",
    color: "white",
    marginBottom: "12px",
  },
  locationActionSubtitle: {
    fontSize: "17px",
    color: "#C0C0C0",
    marginBottom: "24px",
  },
  locationButtonsContainer: {
    display: "flex",
    justifyContent: "center",
    gap: "15px",
    flexWrap: "wrap",
    marginBottom: "20px",
  },
  locationButton: {
    padding: "12px 20px",
    borderRadius: "8px",
    color: "white",
    fontWeight: "500",
    fontSize: "16px",
    border: "none",
    cursor: "pointer",
    transition: "background-color 0.2s ease, transform 0.1s ease",
    minWidth: "180px",
  },
  locationButtonPark: { backgroundColor: "#22C55E" },
  locationButtonParkHover: {
    backgroundColor: "#16A34A",
    transform: "scale(1.05)",
  },
  locationButtonGym: { backgroundColor: "#EF4444" },
  locationButtonGymHover: {
    backgroundColor: "#DC2626",
    transform: "scale(1.05)",
  },
  locationButtonPool: { backgroundColor: "#3B82F6" },
  locationButtonPoolHover: {
    backgroundColor: "#2563EB",
    transform: "scale(1.05)",
  },
  locationStatusText: {
    marginTop: "15px",
    fontSize: "15px",
    color: "#A0AEC0",
    minHeight: "22px",
  },
  mapLinkButton: {
    display: "inline-block",
    marginTop: "15px",
    padding: "10px 20px",
    borderRadius: "8px",
    backgroundColor: "#FF7F50",
    color: "white",
    fontWeight: "600",
    fontSize: "16px",
    textDecoration: "none",
    transition: "background-color 0.2s ease, transform 0.1s ease",
  },
  mapLinkButtonHover: { backgroundColor: "#FF6347", transform: "scale(1.03)" },
  activitiesTitle: {
    fontWeight: "600",
    fontSize: "20px",
    marginBottom: "18px",
    color: "white",
    marginTop: "28px",
    textAlign: "center",
  },
  activityRow: {
    display: "flex",
    alignItems: "center",
    backgroundColor: "#353A47",
    borderRadius: "10px",
    padding: "16px",
    marginBottom: "12px",
    transition: "transform 0.2s ease, background-color 0.2s ease",
  },
  activityRowHover: { transform: "scale(1.02)", backgroundColor: "#404652" },
  activityIcon: {
    marginRight: "18px",
    fontSize: "24px",
    width: "28px",
    textAlign: "center",
  },
  activityName: { flex: 1, color: "#EAEAEA", fontSize: "17px" },
  activityCalories: { fontSize: "15px", color: "#B0B0B0" },
  footerTips: { marginTop: "15px", fontSize: "14px", color: "#A0AEC0" },
  lastCalcInfoSection: {
    padding: "15px",
    backgroundColor: "rgba(255, 255, 255, 0.04)",
    borderRadius: "10px",
    marginBottom: "25px",
    textAlign: "center",
    fontSize: "15px",
    color: "#B0B0B0",
    border: "1px solid #4A505C",
  },
  plannedWorkoutSection: {
    marginTop: "28px",
    padding: "24px",
    backgroundColor: "rgba(102, 205, 170, 0.05)",
    borderRadius: "12px",
    border: "1px dashed #66CDAA",
  },
  plannedWorkoutTitle: {
    fontSize: "20px",
    fontWeight: "600",
    color: "#66CDAA",
    marginBottom: "16px",
    textAlign: "center",
  },
  plannedWorkoutForm: {
    display: "grid",
    gridTemplateColumns: "1fr 1fr auto",
    gap: "15px",
    alignItems: "center",
    marginBottom: "15px",
  },
  plannedWorkoutResult: {
    fontSize: "18px",
    fontWeight: "500",
    color: "#87CEEB",
    textAlign: "center",
    marginTop: "10px",
  },
  dailyGoalSection: {
    marginTop: "25px",
    padding: "15px",
    backgroundColor: "rgba(255, 215, 0, 0.05)",
    borderRadius: "10px",
    textAlign: "center",
  },
  goalCheckboxLabel: {
    fontSize: "17px",
    color: "#FFD700",
    cursor: "pointer",
    display: "flex",
    alignItems: "center",
    justifyContent: "center",
  },
  goalCheckbox: {
    marginRight: "10px",
    width: "18px",
    height: "18px",
    accentColor: "#FFD700",
  },
  favoriteButton: {
    marginLeft: "auto",
    padding: "8px 10px",
    backgroundColor: "transparent",
    border: "none",
    color: "#FFD700",
    cursor: "pointer",
    fontSize: "20px",
    transition: "transform 0.2s ease",
  },
  favoriteButtonHover: { transform: "scale(1.2)" },
  favoriteSection: {
    marginTop: "30px",
    paddingTop: "20px",
    borderTop: "1px solid #505662",
  },
  favoriteTitle: {
    fontWeight: "600",
    fontSize: "20px",
    marginBottom: "18px",
    color: "#FFD700",
    textAlign: "center",
  },
  noFavoritesText: {
    textAlign: "center",
    color: "#A0AEC0",
    fontStyle: "italic",
    fontSize: "16px",
  },
  weightTrackingSection: {
    marginTop: "30px",
    padding: "24px",
    backgroundColor: "rgba(135, 206, 250, 0.07)",
    borderRadius: "12px",
    border: "1px dashed #87CEFA",
  },
  weightTrackingTitle: {
    fontSize: "20px",
    fontWeight: "600",
    color: "#87CEFA",
    marginBottom: "18px",
    textAlign: "center",
  },
  weightInputForm: {
    display: "flex",
    gap: "15px",
    alignItems: "center",
    marginBottom: "20px",
  },
  weightInput: {
    flexGrow: 1,
    padding: "14px",
    border: "1px solid #505662",
    backgroundColor: "#2A2E37",
    color: "#EAEAEA",
    borderRadius: "8px",
    fontSize: "16px",
  },
  weightAddButton: {
    padding: "14px 20px",
    backgroundColor: "#87CEFA",
    color: "#2A2E37",
    border: "none",
    borderRadius: "8px",
    fontWeight: "600",
    cursor: "pointer",
    transition: "background-color 0.2s ease",
  },
  weightAddButtonHover: { backgroundColor: "#6495ED" },
  weightHistoryList: {
    listStyle: "none",
    padding: 0,
    maxHeight: "200px",
    overflowY: "auto",
    marginBottom: "20px",
  },
  weightHistoryItem: {
    display: "flex",
    justifyContent: "space-between",
    alignItems: "center",
    padding: "10px",
    backgroundColor: "rgba(255, 255, 255, 0.03)",
    borderRadius: "6px",
    marginBottom: "8px",
    fontSize: "15px",
  },
  weightHistoryDate: { color: "#B0B0B0" },
  weightHistoryValue: { fontWeight: "500", color: "#EAEAEA" },
  weightDeleteButton: {
    backgroundColor: "transparent",
    border: "none",
    color: "#FF6B6B",
    cursor: "pointer",
    fontSize: "16px",
    transition: "color 0.2s ease",
  },
  weightDeleteButtonHover: { color: "#FF0000" },
  noWeightHistoryText: {
    textAlign: "center",
    color: "#A0AEC0",
    fontStyle: "italic",
    fontSize: "16px",
    marginTop: "10px",
  },
  achievementsSection: {
    marginTop: "30px",
    padding: "24px",
    backgroundColor: "rgba(255, 165, 0, 0.07)",
    borderRadius: "12px",
    border: "1px dashed #FFA500",
  },
  achievementsTitle: {
    fontSize: "20px",
    fontWeight: "600",
    color: "#FFA500",
    marginBottom: "18px",
    textAlign: "center",
  },
  streakInfo: {
    textAlign: "center",
    fontSize: "18px",
    fontWeight: "500",
    color: "#FFD700",
    marginBottom: "15px",
    padding: "10px",
    backgroundColor: "rgba(255,215,0,0.1)",
    borderRadius: "8px",
  },
  achievementList: { listStyle: "none", padding: 0 },
  achievementItem: {
    display: "flex",
    alignItems: "center",
    padding: "12px",
    backgroundColor: "rgba(255, 255, 255, 0.04)",
    borderRadius: "8px",
    marginBottom: "10px",
    transition: "transform 0.2s ease",
  },
  achievementItemHover: { transform: "scale(1.02)" },
  achievementIcon: {
    fontSize: "24px",
    color: "#FFA500",
    marginRight: "15px",
    width: "30px",
    textAlign: "center",
  },
  achievementDetails: { flex: 1 },
  achievementName: {
    fontSize: "17px",
    fontWeight: "600",
    color: "#EAEAEA",
    marginBottom: "4px",
  },
  achievementDescription: { fontSize: "14px", color: "#C0C0C0" },
  noAchievementsText: {
    textAlign: "center",
    color: "#A0AEC0",
    fontStyle: "italic",
    fontSize: "16px",
    marginTop: "10px",
  },
  weightChartContainer: {
    marginTop: "25px",
    padding: "20px",
    backgroundColor: "rgba(255, 255, 255, 0.03)",
    borderRadius: "12px",
    border: "1px solid #505662",
  },
  weightChartTitle: {
    fontSize: "18px",
    fontWeight: "600",
    color: "#87CEFA",
    marginBottom: "15px",
    textAlign: "center",
  },
  waterTrackingSection: {
    marginTop: "30px",
    padding: "24px",
    backgroundColor: "rgba(93, 173, 226, 0.07)",
    borderRadius: "12px",
    border: "1px dashed #5DADE2",
  },
  waterTrackingTitle: {
    fontSize: "20px",
    fontWeight: "600",
    color: "#5DADE2",
    marginBottom: "18px",
    textAlign: "center",
  },
  waterDisplay: {
    textAlign: "center",
    fontSize: "22px",
    fontWeight: "500",
    color: "#EAEAEA",
    marginBottom: "15px",
  },
  waterVisual: {
    display: "flex",
    justifyContent: "center",
    alignItems: "center",
    gap: "8px",
    marginBottom: "20px",
    fontSize: "28px",
  },
  waterGlassIconFilled: { color: "#5DADE2" },
  waterGlassIconEmpty: { color: "#505662" },
  waterControls: {
    display: "flex",
    justifyContent: "center",
    gap: "15px",
  },
  waterButton: {
    padding: "12px 20px",
    borderRadius: "8px",
    color: "#2A2E37",
    fontWeight: "600",
    fontSize: "16px",
    border: "none",
    cursor: "pointer",
    transition: "background-color 0.2s ease, transform 0.1s ease",
    minWidth: "120px",
  },
  waterAddButton: { backgroundColor: "#5DADE2" },
  waterAddButtonHover: { backgroundColor: "#3498DB", transform: "scale(1.05)" },
  waterRemoveButton: { backgroundColor: "#AAB7B8" },
  waterRemoveButtonHover: {
    backgroundColor: "#95A5A6",
    transform: "scale(1.05)",
  },
  menuGeneratorSection: {
    marginTop: "30px",
    padding: "24px",
    backgroundColor: "rgba(147, 112, 219, 0.07)",
    borderRadius: "12px",
    border: "1px dashed #9370DB",
  },
  menuGeneratorTitle: {
    fontSize: "20px",
    fontWeight: "600",
    color: "#9370DB",
    marginBottom: "18px",
    textAlign: "center",
  },
  menuInput: {
    padding: "16px",
    border: "1px solid #505662",
    backgroundColor: "#2A2E37",
    color: "#EAEAEA",
    borderRadius: "10px",
    fontSize: "17px",
    WebkitBoxShadow: "0 0 0 30px #2A2E37 inset !important",
    boxShadow: "0 0 0 30px #2A2E37 inset !important",
    WebkitTextFillColor: "#EAEAEA !important",
    marginBottom: "15px",
    width: "100%",
    boxSizing: "border-box",
  },
  menuButton: {
    gridColumn: "span 2",
    marginTop: "0px",
    padding: "18px",
    borderRadius: "10px",
    color: "white",
    fontWeight: "600",
    fontSize: "18px",
    border: "none",
    cursor: "pointer",
    transition:
      "background-color 0.2s ease-in-out, transform 0.15s ease-in-out, box-shadow 0.2s ease",
    textAlign: "center",
    backgroundColor: "#9370DB",
    width: "100%",
  },
  menuButtonHover: {
    backgroundColor: "#8A2BE2",
    transform: "scale(1.03) translateY(-2px)",
    boxShadow: "0 6px 12px rgba(138, 43, 226, 0.35)",
  },
  menuResultCard: {
    marginTop: "20px",
    padding: "18px",
    backgroundColor: "rgba(255, 255, 255, 0.03)",
    borderRadius: "10px",
    border: "1px solid #4A505C",
  },
  menuMealItem: {
    marginBottom: "10px",
    color: "#EAEAEA",
    fontSize: "16px",
    display: "flex",
    alignItems: "center",
  },
  menuMealIcon: {
    marginRight: "10px",
    color: "#9370DB",
    fontSize: "18px",
  },
  menuMealName: {
    fontWeight: "500",
    color: "#E0E0E0",
  },
  menuMealCalories: {
    marginLeft: "auto",
    color: "#B0B0B0",
    fontSize: "15px",
  },
  menuTotalCalories: {
    textAlign: "right",
    fontWeight: "600",
    fontSize: "17px",
    color: "#9370DB",
    marginTop: "15px",
    paddingTop: "10px",
    borderTop: "1px solid #505662",
  },
  menuDisclaimer: {
    fontSize: "13px",
    color: "#A0AEC0",
    fontStyle: "italic",
    textAlign: "center",
    marginTop: "15px",
  },
  recipeFinderSection: {
    marginTop: "30px",
    padding: "24px",
    backgroundColor: "rgba(255, 165, 0, 0.07)",
    borderRadius: "12px",
    border: "1px dashed #FFA500",
  },
  recipeFinderTitle: {
    fontSize: "20px",
    fontWeight: "600",
    color: "#FFA500",
    marginBottom: "18px",
    textAlign: "center",
  },
  ingredientsTextarea: {
    padding: "16px",
    border: "1px solid #505662",
    backgroundColor: "#2A2E37",
    color: "#EAEAEA",
    borderRadius: "10px",
    fontSize: "17px",
    WebkitBoxShadow: "0 0 0 30px #2A2E37 inset !important",
    boxShadow: "0 0 0 30px #2A2E37 inset !important",
    WebkitTextFillColor: "#EAEAEA !important",
    width: "100%",
    minHeight: "80px",
    marginBottom: "15px",
    boxSizing: "border-box",
    resize: "vertical",
  },
  recipeButton: {
    gridColumn: "span 2",
    marginTop: "0px",
    padding: "18px",
    borderRadius: "10px",
    color: "white",
    fontWeight: "600",
    fontSize: "18px",
    border: "none",
    cursor: "pointer",
    transition:
      "background-color 0.2s ease-in-out, transform 0.15s ease-in-out, box-shadow 0.2s ease",
    textAlign: "center",
    backgroundColor: "#FFA500",
    width: "100%",
  },
  recipeButtonHover: {
    backgroundColor: "#FF8C00",
    transform: "scale(1.03) translateY(-2px)",
    boxShadow: "0 6px 12px rgba(255, 140, 0, 0.35)",
  },
  recipeResultCard: {
    marginTop: "20px",
    padding: "18px",
    backgroundColor: "rgba(255, 255, 255, 0.03)",
    borderRadius: "10px",
    border: "1px solid #4A505C",
  },
  recipeName: {
    fontSize: "19px",
    fontWeight: "600",
    color: "#FFA500",
    marginBottom: "12px",
    textAlign: "center",
  },
  recipeImage: {
    width: "100%",
    maxHeight: "220px",
    objectFit: "cover",
    borderRadius: "8px",
    marginBottom: "15px",
    border: "1px solid #505662",
  },
  recipeSectionTitle: {
    fontSize: "17px",
    fontWeight: "600",
    color: "#E0E0E0",
    marginTop: "15px",
    marginBottom: "8px",
  },
  recipeIngredientsList: {
    listStyle: "disc",
    listStylePosition: "inside",
    paddingLeft: "5px",
    color: "#C0C0C0",
    fontSize: "15px",
    lineHeight: "1.6",
  },
  recipeInstructions: {
    whiteSpace: "pre-line",
    color: "#C0C0C0",
    fontSize: "15px",
    lineHeight: "1.7",
  },
};

const ALL_ACHIEVEMENTS = {
  firstCalc: {
    id: "firstCalc",
    name: "İlk Adım!",
    description: "İlk kalori hedefini başarıyla hesapladın.",
    icon: "fa-calculator",
  },
  firstWeightEntry: {
    id: "firstWeightEntry",
    name: "Takip Başladı!",
    description: "İlk kilo kaydını girdin.",
    icon: "fa-weight-scale",
  },
  streak3Days: {
    id: "streak3Days",
    name: "İstikrar Abidesi (3 Gün)!",
    description: "3 gün üst üste günlük kalori hedefine ulaştın.",
    icon: "fa-medal",
  },
  streak7Days: {
    id: "streak7Days",
    name: "Süper Seri (7 Gün)!",
    description: "7 gün üst üste günlük kalori hedefine ulaştın.",
    icon: "fa-trophy",
  },
  firstFavorite: {
    id: "firstFavorite",
    name: "Favori Avcısı!",
    description: "İlk favori aktiviteni ekledin.",
    icon: "fa-star",
  },
  threeFavorites: {
    id: "threeFavorites",
    name: "Koleksiyoner!",
    description: "3 farklı favori aktivite ekledin.",
    icon: "fa-grin-stars",
  },
  firstLocationSearch: {
    id: "firstLocationSearch",
    name: "Kaşif Ruhlu!",
    description: "İlk kez konum tabanlı aktivite yeri aradın.",
    icon: "fa-map-marked-alt",
  },
  firstPlannedWorkout: {
    id: "firstPlannedWorkout",
    name: "Planlı Programlı!",
    description: "İlk kez planladığın bir antrenmanın kalorisini tahmin ettin.",
    icon: "fa-clipboard-check",
  },
  weightMilestone5Entries: {
    id: "weightMilestone5Entries",
    name: "Kilo Günlüğü Uzmanı!",
    description: "Kilo takip geçmişine 5 kayıt ekledin.",
    icon: "fa-book-medical",
  },
  waterGoalMet: {
    id: "waterGoalMet",
    name: "Su Perisi!",
    description: "Günlük su hedefine ulaştın. Harika!",
    icon: "fa-tint",
  },
  // --- YENİ ÖZELLİKLER İÇİN BAŞARIMLAR (FAZ 9) ---
  firstMenuGenerated: {
    id: "firstMenuGenerated",
    name: "Menü Şefi!",
    description: "İlk kaloriye uygun yemek menünü oluşturdun.",
    icon: "fa-utensils",
  },
  firstRecipeFound: {
    id: "firstRecipeFound",
    name: "Mutfak Kaşifi!",
    description: "Malzemelerinle ilk yemek tarifini buldun.",
    icon: "fa-lightbulb",
  },
};

function calculateCalories({ gender, age, weight, height, activity }) {
  let bmr =
    gender === "female"
      ? 10 * weight + 6.25 * height - 5 * age - 161
      : 10 * weight + 6.25 * height - 5 * age + 5;
  const activityMultipliers = {
    sedentary: 1.2,
    light: 1.375,
    moderate: 1.55,
    active: 1.725,
    veryActive: 1.9,
  };
  return Math.round(bmr * (activityMultipliers[activity] || 1.2));
}
const activities = [
  {
    name: "Yürüyüş (Normal Tempo)",
    icon: "fa-person-walking",
    calories: 200,
    met: 3.5,
  },
  {
    name: "Tempolu Yürüyüş",
    icon: "fa-person-hiking",
    calories: 300,
    met: 4.3,
  },
  {
    name: "Koşu (Hafif Tempo)",
    icon: "fa-person-running",
    calories: 400,
    met: 7.0,
  },
  {
    name: "Koşu (Orta Tempo)",
    icon: "fa-person-running",
    calories: 550,
    met: 9.8,
  },
  {
    name: "Bisiklet (Düz Yol)",
    icon: "fa-person-biking",
    calories: 350,
    met: 5.5,
  },
  {
    name: "Bisiklet (Yokuşlu)",
    icon: "fa-motorcycle",
    calories: 500,
    met: 8.5,
  },
  { name: "Ağırlık Antrenmanı", icon: "fa-dumbbell", calories: 300, met: 5.0 },
  {
    name: "Yüzme (Serbest Stil)",
    icon: "fa-person-swimming",
    calories: 450,
    met: 8.3,
  },
  { name: "Yoga / Pilates", icon: "fa-spa", calories: 180, met: 2.5 },
  {
    name: "Dans (Zumba vb.)",
    icon: "fa-compact-disc",
    calories: 380,
    met: 6.8,
  },
  { name: "Ev Temizliği (Yoğun)", icon: "fa-broom", calories: 250, met: 3.8 },
  { name: "Bahçe İşleri", icon: "fa-leaf", calories: 220, met: 3.0 },
  { name: "Merdiven Çıkma", icon: "fa-stairs", calories: 480, met: 8.0 },
];
function generateWorkoutPlan(activityLevel, gender) {
  const plans = {
    sedentary: [
      { name: "Kısa bir yürüyüş yap (15-20 dk)", icon: "fa-person-walking" },
      { name: "Hafif esneme hareketleri yap", icon: "fa-hands-praying" },
      { name: "Asansör yerine merdiven kullan", icon: "fa-stairs" },
    ],
    light: [
      { name: "30 dakikalık tempolu yürüyüş", icon: "fa-person-hiking" },
      { name: "15-20 dakika yoga veya pilates", icon: "fa-spa" },
      { name: "Hafif tempolu bisiklete bin (20 dk)", icon: "fa-person-biking" },
    ],
    moderate: [
      {
        name: "30-45 dakika koşu veya tempolu bisiklet",
        icon: "fa-person-running",
      },
      { name: "30 dakika yüzme", icon: "fa-person-swimming" },
      { name: "Ağırlık antrenmanı (30-40 dk)", icon: "fa-dumbbell" },
    ],
    active: [
      { name: "60 dakika yoğun antrenman (koşu, HIIT)", icon: "fa-bolt" },
      { name: "Uzun mesafe bisiklet veya yüzme", icon: "fa-person-biking" },
      { name: "Takım sporlarına katıl", icon: "fa-futbol" },
    ],
    veryActive: [
      { name: "Profesyonel düzeyde antrenman veya yarışma", icon: "fa-trophy" },
      { name: "Günde birden fazla yoğun antrenman seansı", icon: "fa-fire" },
      { name: "Çok zorlu fiziksel aktiviteler", icon: "fa-mountain" },
    ],
  };
  const selectedPlan = plans[activityLevel] || plans.light;
  return selectedPlan[Math.floor(Math.random() * selectedPlan.length)];
}
function getNutritionGuidance(calories) {
  let guidance = {
    title: "Genel Beslenme Önerileri",
    points: [
      "Bol su içmeyi unutma (günde en az 2-2.5 litre).",
      "Her öğünde protein, karbonhidrat ve sağlıklı yağ dengesine dikkat et.",
      "İşlenmiş gıdalardan, şekerli içeceklerden ve aşırı tuzdan uzak dur.",
      "Mevsiminde taze sebze ve meyve tüketmeye özen göster.",
      "Porsiyon kontrolüne dikkat et, yavaş ye ve doyduğunu hissettiğinde bırak.",
    ],
    disclaimer:
      "Bu öneriler genel bilgilendirme amaçlıdır. Kişisel sağlık durumunuz ve özel ihtiyaçlarınız için bir diyetisyen veya doktora danışmanız en doğrusudur.",
  };
  if (calories < 1500) {
    guidance.title = "Düşük Kalori Alımı İçin Beslenme İpuçları";
    guidance.points.unshift(
      "Kalorisi düşük ancak besin değeri yüksek yiyecekleri tercih et (yeşil yapraklı sebzeler, yağsız proteinler)."
    );
    guidance.points.push(
      "Ara öğünlerde sağlıklı atıştırmalıklar (meyve, yoğurt, kuruyemiş) tüketebilirsin."
    );
  } else if (calories > 2500) {
    guidance.title = "Yüksek Kalori Alımı İçin Beslenme Stratejileri";
    guidance.points.unshift(
      "Enerji yoğunluğu yüksek, sağlıklı gıdalar tüket (kuruyemişler, avokado, tam tahıllar)."
    );
    guidance.points.push(
      "Kas gelişimi için yeterli protein aldığından emin ol."
    );
  }
  return guidance;
}
function calculatePlannedWorkoutCalories(
  activityName,
  durationMinutes,
  weightKg
) {
  if (
    !activityName ||
    !durationMinutes ||
    !weightKg ||
    durationMinutes <= 0 ||
    weightKg <= 0
  )
    return 0;
  const activity = activities.find((act) => act.name === activityName);
  const met = activity ? activity.met : 3.5;
  return Math.round(((met * 3.5 * weightKg) / 200) * durationMinutes);
}

function ActivityRow({ activity, gender, isFavorite, onToggleFavorite }) {
  const [isHovered, setIsHovered] = useState(false);
  const [isFavButtonHovered, setIsFavButtonHovered] = useState(false);
  const handleFavoriteClick = (e) => {
    e.stopPropagation();
    onToggleFavorite(activity.name);
  };
  return (
    <div
      style={
        isHovered
          ? { ...styles.activityRow, ...styles.activityRowHover }
          : styles.activityRow
      }
      onMouseEnter={() => setIsHovered(true)}
      onMouseLeave={() => setIsHovered(false)}
    >
      <i
        className={`fa-solid ${activity.icon}`}
        style={{
          ...styles.activityIcon,
          color: gender === "female" ? "#FF69B4" : "#1E90FF",
        }}
      ></i>
      <span style={styles.activityName}>{activity.name}</span>
      <span style={styles.activityCalories}>{activity.calories} kcal/saat</span>
      <button
        style={
          isFavButtonHovered
            ? { ...styles.favoriteButton, ...styles.favoriteButtonHover }
            : styles.favoriteButton
        }
        onClick={handleFavoriteClick}
        onMouseEnter={() => setIsFavButtonHovered(true)}
        onMouseLeave={() => setIsFavButtonHovered(false)}
        title={isFavorite ? "Favorilerden Çıkar" : "Favorilere Ekle"}
      >
        <i className={`fa-${isFavorite ? "solid" : "regular"} fa-star`}></i>
      </button>
    </div>
  );
}

function WeightChart({ weightHistory, gender }) {
  const chartRef = useRef(null);
  const chartInstanceRef = useRef(null);

  useEffect(() => {
    if (typeof Chart === "undefined") return;
    if (!chartRef.current) return;

    if (!weightHistory || weightHistory.length < 2) {
      if (chartInstanceRef.current) {
        chartInstanceRef.current.destroy();
        chartInstanceRef.current = null;
      }
      return;
    }

    const ctx = chartRef.current.getContext("2d");
    if (!ctx) return;

    if (chartInstanceRef.current) {
      chartInstanceRef.current.destroy();
      chartInstanceRef.current = null;
    }

    try {
      const sortedHistory = [...weightHistory].sort((a, b) => {
        const dateA = new Date(String(a.date).split(".").reverse().join("-"));
        const dateB = new Date(String(b.date).split(".").reverse().join("-"));
        if (isNaN(dateA.getTime()) || isNaN(dateB.getTime())) return 0;
        return dateA - dateB;
      });

      const labels = sortedHistory.map((entry) => entry.date);
      const dataPoints = sortedHistory.map((entry) => entry.weight);

      if (labels.length === 0 || dataPoints.length === 0) return;

      const chartColor =
        gender === "female"
          ? styles.buttonFemale.backgroundColor
          : styles.buttonMale.backgroundColor;
      const gridColor = "rgba(160, 174, 192, 0.1)";
      const textColor = "#A0AEC0";

      chartInstanceRef.current = new Chart(ctx, {
        type: "line",
        data: {
          labels: labels,
          datasets: [
            {
              label: "Kilo (kg)",
              data: dataPoints,
              borderColor: chartColor,
              backgroundColor: chartColor
                .replace(")", ", 0.2)")
                .replace("rgb", "rgba"),
              tension: 0.2,
              fill: true,
              pointBackgroundColor: chartColor,
              pointBorderColor: "#FFFFFF",
              pointHoverBackgroundColor: "#FFFFFF",
              pointHoverBorderColor: chartColor,
              pointRadius: 4,
              pointHoverRadius: 6,
            },
          ],
        },
        options: {
          responsive: true,
          maintainAspectRatio: false,
          plugins: {
            legend: { display: false },
            tooltip: {
              backgroundColor: "#2A2E37",
              titleColor: "#EAEAEA",
              bodyColor: "#EAEAEA",
              borderColor: chartColor,
              borderWidth: 1,
              padding: 10,
              cornerRadius: 6,
              callbacks: {
                label: function (context) {
                  return ` Kilo: ${context.parsed.y.toFixed(1)} kg`;
                },
              },
            },
          },
          scales: {
            x: {
              ticks: {
                color: textColor,
                font: { family: "'Poppins', sans-serif" },
              },
              grid: { color: gridColor, borderColor: gridColor },
            },
            y: {
              ticks: {
                color: textColor,
                font: { family: "'Poppins', sans-serif" },
                callback: function (value) {
                  return value.toFixed(1) + " kg";
                },
              },
              grid: { color: gridColor, borderColor: gridColor },
              beginAtZero: false,
              grace: "10%",
            },
          },
        },
      });
    } catch (error) {
      console.error("[WeightChart] Error creating chart:", error);
    }

    return () => {
      if (chartInstanceRef.current) {
        chartInstanceRef.current.destroy();
        chartInstanceRef.current = null;
      }
    };
  }, [weightHistory, gender]);

  if (!weightHistory || weightHistory.length < 2) {
    return (
      <p
        style={{
          ...styles.noWeightHistoryText,
          marginTop: "15px",
          color: styles.weightChartTitle.color,
        }}
      >
        Grafiği görmek için en az 2 kilo kaydı eklemelisin.
      </p>
    );
  }

  return (
    <div style={styles.weightChartContainer}>
      <h4 style={styles.weightChartTitle}>
        <i className="fas fa-chart-line" style={{ marginRight: "8px" }}></i>Kilo
        Değişim Grafiğin
      </h4>
      <div style={{ position: "relative", height: "300px", width: "100%" }}>
        <canvas ref={chartRef}></canvas>
      </div>
    </div>
  );
}

function CalorieCalculator({ gender }) {
  const [inputs, setInputs] = useState({
    age: "",
    weight: "",
    height: "",
    activity: "sedentary",
  });
  const [result, setResult] = useState(null);
  const [workoutPlan, setWorkoutPlan] = useState(null);
  const [isCalcButtonHovered, setIsCalcButtonHovered] = useState(false);
  const [userLocation, setUserLocation] = useState(null);
  const [locationStatus, setLocationStatus] = useState("idle");
  const [mapSearchQuery, setMapSearchQuery] = useState("");
  const [hoveredLocationButton, setHoveredLocationButton] = useState("");
  const [nutritionInfo, setNutritionInfo] = useState(null);
  const [plannedWorkout, setPlannedWorkout] = useState({
    type: activities[0].name,
    duration: 30,
  });
  const [estimatedWorkoutBurn, setEstimatedWorkoutBurn] = useState(null);
  const [lastCalcData, setLastCalcData] = useState(null);
  const [dailyGoalCompleted, setDailyGoalCompleted] = useState(false);
  const [favoriteActivities, setFavoriteActivities] = useState([]);
  const [currentWeightInput, setCurrentWeightInput] = useState("");
  const [weightHistory, setWeightHistory] = useState([]);
  const [isWeightAddButtonHovered, setIsWeightAddButtonHovered] =
    useState(false);
  const [achievements, setAchievements] = useState([]);
  const [dailyGoalStreak, setDailyGoalStreak] = useState(0);
  const [lastGoalMetDate, setLastGoalMetDate] = useState(null);
  const [waterGoal] = useState(8);
  const [waterConsumed, setWaterConsumed] = useState(0);
  const [waterUnit] = useState("bardak");
  const [isWaterAddHovered, setIsWaterAddHovered] = useState(false);
  const [isWaterRemoveHovered, setIsWaterRemoveHovered] = useState(false);

  // --- YENİ ÖZELLİKLER İÇİN STATE'LER (FAZ 9) ---
  const [targetMenuCalories, setTargetMenuCalories] = useState("");
  const [generatedMenu, setGeneratedMenu] = useState(null);
  const [userIngredients, setUserIngredients] = useState("");
  const [generatedRecipe, setGeneratedRecipe] = useState(null);
  const [isMenuButtonHovered, setIsMenuButtonHovered] = useState(false);
  const [isRecipeButtonHovered, setIsRecipeButtonHovered] = useState(false);

  const localStorageKeyBase = `calorieApp_${gender}`;
  const lastCalcStorageKey = `${localStorageKeyBase}_lastCalc`;
  const dailyGoalCheckboxStorageKey = `${localStorageKeyBase}_dailyGoalCheckbox_${
    new Date().toISOString().split("T")[0]
  }`;
  const favoriteActivitiesStorageKey = `${localStorageKeyBase}_favoriteActivities`;
  const weightHistoryStorageKey = `${localStorageKeyBase}_weightHistory`;
  const achievementsStorageKey = `${localStorageKeyBase}_achievements`;
  const dailyGoalStreakStorageKey = `${localStorageKeyBase}_dailyGoalStreak`;
  const lastGoalMetDateStorageKey = `${localStorageKeyBase}_lastGoalMetDate`;
  const waterConsumedStorageKey = `${localStorageKeyBase}_waterConsumed_${
    new Date().toISOString().split("T")[0]
  }`;

  const unlockAchievement = (achievementId) => {
    setAchievements((prevAchievements) => {
      if (!prevAchievements.includes(achievementId)) {
        const newAchievements = [...prevAchievements, achievementId];
        localStorage.setItem(
          achievementsStorageKey,
          JSON.stringify(newAchievements)
        );
        return newAchievements;
      }
      return prevAchievements;
    });
  };

  useEffect(() => {
    const storedLastCalc = localStorage.getItem(lastCalcStorageKey);
    if (storedLastCalc) setLastCalcData(JSON.parse(storedLastCalc));

    const storedGoalCheckboxStatus = localStorage.getItem(
      dailyGoalCheckboxStorageKey
    );
    if (storedGoalCheckboxStatus)
      setDailyGoalCompleted(JSON.parse(storedGoalCheckboxStatus));

    const storedFavoriteActivities = localStorage.getItem(
      favoriteActivitiesStorageKey
    );
    if (storedFavoriteActivities)
      setFavoriteActivities(JSON.parse(storedFavoriteActivities));

    const storedWeightHistory = localStorage.getItem(weightHistoryStorageKey);
    if (storedWeightHistory) setWeightHistory(JSON.parse(storedWeightHistory));

    const storedAchievements = localStorage.getItem(achievementsStorageKey);
    if (storedAchievements) setAchievements(JSON.parse(storedAchievements));

    const storedStreak = localStorage.getItem(dailyGoalStreakStorageKey);
    if (storedStreak) setDailyGoalStreak(parseInt(storedStreak, 10));

    const storedLastGoalDate = localStorage.getItem(lastGoalMetDateStorageKey);
    if (storedLastGoalDate) setLastGoalMetDate(storedLastGoalDate);

    const storedWaterConsumed = localStorage.getItem(waterConsumedStorageKey);
    if (storedWaterConsumed) {
      setWaterConsumed(parseInt(storedWaterConsumed, 10));
    }
  }, [
    gender,
    lastCalcStorageKey,
    dailyGoalCheckboxStorageKey,
    favoriteActivitiesStorageKey,
    weightHistoryStorageKey,
    achievementsStorageKey,
    dailyGoalStreakStorageKey,
    lastGoalMetDateStorageKey,
    waterConsumedStorageKey,
  ]);

  const handleChange = (e) => {
    setInputs({ ...inputs, [e.target.name]: e.target.value });
    setResult(null);
    setWorkoutPlan(null);
    setNutritionInfo(null);
    setEstimatedWorkoutBurn(null);
    setGeneratedMenu(null);
    setGeneratedRecipe(null);
  };
  const handleCalculate = (e) => {
    e.preventDefault();
    const { age, weight, height, activity } = inputs;
    if (!age || !weight || !height) {
      setResult(null);
      setWorkoutPlan(null);
      setNutritionInfo(null);
      return;
    }
    const calculatedCalories = calculateCalories({
      gender,
      age: Number(age),
      weight: Number(weight),
      height: Number(height),
      activity,
    });
    setResult(calculatedCalories);
    setWorkoutPlan(generateWorkoutPlan(activity, gender));
    setNutritionInfo(getNutritionGuidance(calculatedCalories));
    const newLastCalcData = {
      date: new Date().toLocaleDateString("tr-TR"),
      calories: calculatedCalories,
      weight: inputs.weight,
      activity: inputs.activity,
    };
    localStorage.setItem(lastCalcStorageKey, JSON.stringify(newLastCalcData));
    setLastCalcData(newLastCalcData);
    unlockAchievement("firstCalc");
  };
  const updateDailyGoalStreak = (isGoalMetToday) => {
    const todayStr = new Date().toISOString().split("T")[0];
    if (isGoalMetToday) {
      let newStreak = 1;
      if (lastGoalMetDate) {
        const yesterday = new Date(todayStr);
        yesterday.setDate(yesterday.getDate() - 1);
        const yesterdayStr = yesterday.toISOString().split("T")[0];
        if (lastGoalMetDate === yesterdayStr) {
          newStreak = dailyGoalStreak + 1;
        }
      }
      setDailyGoalStreak(newStreak);
      setLastGoalMetDate(todayStr);
      localStorage.setItem(dailyGoalStreakStorageKey, newStreak.toString());
      localStorage.setItem(lastGoalMetDateStorageKey, todayStr);
      if (newStreak >= 3) unlockAchievement("streak3Days");
      if (newStreak >= 7) unlockAchievement("streak7Days");
    }
  };
  const handleGoalChange = (e) => {
    const isChecked = e.target.checked;
    setDailyGoalCompleted(isChecked);
    localStorage.setItem(
      dailyGoalCheckboxStorageKey,
      JSON.stringify(isChecked)
    );
    updateDailyGoalStreak(isChecked);
  };
  const handlePlannedWorkoutChange = (e) => {
    setPlannedWorkout({ ...plannedWorkout, [e.target.name]: e.target.value });
    setEstimatedWorkoutBurn(null);
  };
  const handleEstimateWorkoutBurn = () => {
    if (!inputs.weight) {
      alert("Lütfen önce ana formda kilonuzu girin.");
      return;
    }
    const burn = calculatePlannedWorkoutCalories(
      plannedWorkout.type,
      Number(plannedWorkout.duration),
      Number(inputs.weight)
    );
    setEstimatedWorkoutBurn(burn);
    unlockAchievement("firstPlannedWorkout");
  };
  const handleLocationRequest = (queryType) => {
    setLocationStatus("loading");
    setMapSearchQuery("");
    if (navigator.geolocation) {
      navigator.geolocation.getCurrentPosition(
        (position) => {
          const { latitude, longitude } = position.coords;
          setUserLocation({ latitude, longitude });
          setLocationStatus("success");
          let query = "";
          if (queryType === "park") query = "yakındaki parklar";
          else if (queryType === "gym") query = "yakındaki spor salonları";
          else if (queryType === "pool") query = "yakındaki yüzme havuzları";
          setMapSearchQuery(
            `https://www.google.com/maps/search/${encodeURIComponent(
              query
            )}/@${latitude},${longitude},15z`
          );
          unlockAchievement("firstLocationSearch");
        },
        (error) => {
          setLocationStatus("error");
          setMapSearchQuery("");
        }
      );
    } else {
      setLocationStatus("unsupported");
      setMapSearchQuery("");
    }
  };
  const toggleFavoriteActivity = (activityName) => {
    setFavoriteActivities((prevFavorites) => {
      const isAlreadyFavorite = prevFavorites.includes(activityName);
      let updatedFavorites;
      if (isAlreadyFavorite) {
        updatedFavorites = prevFavorites.filter(
          (name) => name !== activityName
        );
      } else {
        updatedFavorites = [...prevFavorites, activityName];
        if (updatedFavorites.length === 1) unlockAchievement("firstFavorite");
        if (updatedFavorites.length >= 3) unlockAchievement("threeFavorites");
      }
      localStorage.setItem(
        favoriteActivitiesStorageKey,
        JSON.stringify(updatedFavorites)
      );
      return updatedFavorites;
    });
  };
  const handleCurrentWeightInputChange = (e) => {
    setCurrentWeightInput(e.target.value);
  };
  const handleAddWeightEntry = () => {
    if (
      !currentWeightInput ||
      isNaN(parseFloat(currentWeightInput)) ||
      parseFloat(currentWeightInput) <= 0
    ) {
      alert("Lütfen geçerli bir kilo değeri girin.");
      return;
    }
    const newEntry = {
      date: new Date().toLocaleDateString("tr-TR", {
        day: "2-digit",
        month: "2-digit",
        year: "numeric",
      }),
      weight: parseFloat(currentWeightInput),
    };
    const updatedHistory = [newEntry, ...weightHistory];
    setWeightHistory(updatedHistory);
    localStorage.setItem(
      weightHistoryStorageKey,
      JSON.stringify(updatedHistory)
    );
    setCurrentWeightInput("");
    unlockAchievement("firstWeightEntry");
    if (updatedHistory.length >= 5)
      unlockAchievement("weightMilestone5Entries");
  };
  const handleDeleteWeightEntry = (entryToDelete) => {
    const updatedHistory = weightHistory.filter(
      (entry) =>
        !(
          entry.date === entryToDelete.date &&
          entry.weight === entryToDelete.weight
        )
    );
    setWeightHistory(updatedHistory);
    localStorage.setItem(
      weightHistoryStorageKey,
      JSON.stringify(updatedHistory)
    );
  };
  const handleAddWater = () => {
    setWaterConsumed((prev) => {
      const newAmount = prev + 1;
      localStorage.setItem(waterConsumedStorageKey, newAmount.toString());
      if (newAmount >= waterGoal) {
        unlockAchievement("waterGoalMet");
      }
      return newAmount;
    });
  };
  const handleRemoveWater = () => {
    setWaterConsumed((prev) => {
      const newAmount = Math.max(0, prev - 1);
      localStorage.setItem(waterConsumedStorageKey, newAmount.toString());
      return newAmount;
    });
  };

  // --- YEMEK MENÜSÜ OLUŞTURMA FONKSİYONU (FAZ 9) ---
  const handleGenerateMenu = () => {
    const calories = parseInt(targetMenuCalories);
    if (isNaN(calories) || calories <= 500 || calories > 10000) {
      alert("Lütfen 500 ile 10000 arasında geçerli bir kalori miktarı girin.");
      setGeneratedMenu(null);
      return;
    }

    const mealDatabase = {
      breakfast: [
        {
          name: "Yulaf Ezmesi (Meyve ve Kuruyemişli)",
          calories: 350,
          icon: "fa-bowl-rice",
        },
        {
          name: "Tam Buğday Ekmeği Üzerine Avokado ve 2 Yumurta",
          calories: 450,
          icon: "fa-egg",
        },
        {
          name: "Menemen (2 yumurtalı, peynirli)",
          calories: 400,
          icon: "fa-pepper-hot",
        },
        {
          name: "Yoğurt, Granola ve Orman Meyveleri",
          calories: 300,
          icon: "fa-ice-cream",
        },
      ],
      lunch: [
        { name: "Izgara Tavuk Göğsü Salata", calories: 500, icon: "fa-carrot" },
        {
          name: "Mercimek Çorbası ve Mevsim Salata",
          calories: 450,
          icon: "fa-mortar-pestle",
        },
        {
          name: "Sebzeli Kinoa Salatası (Nohutlu)",
          calories: 400,
          icon: "fa-seedling",
        },
        {
          name: "Ton Balıklı Sandviç (Tam Buğday)",
          calories: 480,
          icon: "fa-fish",
        },
      ],
      dinner: [
        {
          name: "Fırında Somon, Buharda Brokoli",
          calories: 600,
          icon: "fa-drumstick-bite",
        },
        {
          name: "Sebzeli Tavuk Sote ve Bulgur Pilavı",
          calories: 550,
          icon: "fa-plate-wheat",
        },
        {
          name: "Kuru Fasulye Yemeği (Etsiz) ve Cacık",
          calories: 500,
          icon: "fa-bowl-food",
        },
        {
          name: "Izgara Köfte, Közlenmiş Biber ve Salata",
          calories: 580,
          icon: "fa-burger",
        },
      ],
      snacks: [
        { name: "Bir Orta Boy Elma", calories: 95, icon: "fa-apple-whole" },
        {
          name: "Bir Avuç Badem (15-20 adet)",
          calories: 140,
          icon: "fa-candy-cane",
        },
        {
          name: "Bir Kase Sade Yoğurt",
          calories: 120,
          icon: "fa-prescription-bottle",
        },
        { name: "Bir Orta Boy Muz", calories: 105, icon: "fa-lemon" },
      ],
    };

    let menu = {
      breakfast: null,
      lunch: null,
      dinner: null,
      snacks: [],
      totalCalories: 0,
    };
    let remainingCalories = calories;

    const selectMeal = (mealType, targetRatio) => {
      const targetCal = calories * targetRatio;
      const sortedMeals = [...mealDatabase[mealType]].sort(
        (a, b) =>
          Math.abs(a.calories - targetCal) - Math.abs(b.calories - targetCal)
      );
      if (sortedMeals.length > 0) {
        menu[mealType] = sortedMeals[0];
        remainingCalories -= sortedMeals[0].calories;
        menu.totalCalories += sortedMeals[0].calories;
      }
    };

    selectMeal("breakfast", 0.25);
    selectMeal("lunch", 0.3);
    selectMeal("dinner", 0.3);

    mealDatabase.snacks.sort((a, b) => b.calories - a.calories);
    for (const snack of mealDatabase.snacks) {
      if (
        remainingCalories >= snack.calories &&
        menu.snacks.length < 2 &&
        calories - menu.totalCalories > snack.calories * 0.5
      ) {
        menu.snacks.push(snack);
        remainingCalories -= snack.calories;
        menu.totalCalories += snack.calories;
      }
    }
    setGeneratedMenu(menu);
    unlockAchievement("firstMenuGenerated");
  };

  // --- MALZEMEYE GÖRE TARİF BULMA FONKSİYONU (FAZ 9) ---
  const handleGenerateRecipe = () => {
    if (!userIngredients.trim()) {
      alert("Lütfen elinizdeki malzemeleri girin (virgülle ayırarak).");
      setGeneratedRecipe(null);
      return;
    }
    const ingredientsArray = userIngredients
      .toLowerCase()
      .split(",")
      .map((item) => item.trim())
      .filter((item) => item);

    const recipes = [
      {
        name: "Pratik Menemen",
        required: ["yumurta", "domates"],
        optional: [
          "biber",
          "soğan",
          "zeytinyağı",
          "tuz",
          "karabiber",
          "pul biber",
          "peynir",
        ],
        instructions:
          "Soğan ve biberleri zeytinyağında kavurun. Küp doğranmış domatesleri ekleyip suyunu çekene kadar pişirin. Yumurtaları kırın, baharatları ve isteğe bağlı peyniri ekleyip karıştırarak pişirin.",
        image:
          "https://images.unsplash.com/photo-1604503468807-1595c9ce3144?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MXx8bWVuZW1lbnxlbnwwfHwwfHx8MA%3D%3D&auto=format&fit=crop&w=500&q=60",
        icon: "fa-egg",
      },
      {
        name: "Domates Soslu Makarna",
        required: ["makarna", "domates"],
        optional: [
          "soğan",
          "sarımsak",
          "zeytinyağı",
          "fesleğen",
          "kekik",
          "tuz",
          "karabiber",
        ],
        instructions:
          "Makarnayı haşlayın. Ayrı bir tavada soğan ve sarımsağı kavurun. Domatesleri ekleyip pişirin. Baharatları ekleyin. Makarna ile karıştırın.",
        image:
          "https://images.unsplash.com/photo-1598866594240-a7161916037a?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8M3x8cGFzdGElMjB0b21hdG8lMjBzYXVjZXxlbnwwfHwwfHx8MA%3D%3D&auto=format&fit=crop&w=500&q=60",
        icon: "fa-plate-wheat",
      },
      {
        name: "Tavuklu Salata",
        required: ["tavuk", "marul"],
        optional: [
          "domates",
          "salatalık",
          "zeytinyağı",
          "limon",
          "mısır",
          "biber",
        ],
        instructions:
          "Tavuğu haşlayın veya ızgara yapın, doğrayın. Yeşillikleri ve diğer sebzeleri doğrayın. Tüm malzemeleri karıştırıp zeytinyağı ve limon ile soslayın.",
        image:
          "https://plus.unsplash.com/premium_photo-1664478240998-03439a2146a0?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MXx8Y2hpY2tlbiUyMHNhbGFkfGVufDB8fDB8fHww&auto=format&fit=crop&w=500&q=60",
        icon: "fa-carrot",
      },
      {
        name: "Meyveli Yoğurt Kasesi",
        required: ["yoğurt"],
        optional: ["muz", "çilek", "elma", "bal", "granola", "yulaf", "ceviz"],
        instructions:
          "Yoğurdu kaseye alın. Meyveleri doğrayıp ekleyin. İsteğe bağlı bal, granola veya yulaf ekleyebilirsiniz.",
        image:
          "https://images.unsplash.com/photo-1505252585461-04db1eb84625?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8NHx8eW9ndXJ0JTIwYm93bCUyMGZydWl0fGVufDB8fDB8fHww&auto=format&fit=crop&w=500&q=60",
        icon: "fa-apple-whole",
      },
    ];

    let bestMatch = null;
    let maxMatchCount = 0;

    for (const recipe of recipes) {
      let currentMatchCount = 0;
      const availableOptional = [];
      const hasAllRequired = recipe.required.every((reqIngredient) =>
        ingredientsArray.some(
          (userIng) =>
            userIng.includes(reqIngredient) || reqIngredient.includes(userIng)
        )
      );

      if (hasAllRequired) {
        currentMatchCount += recipe.required.length * 2;
        recipe.optional.forEach((optIngredient) => {
          if (
            ingredientsArray.some(
              (userIng) =>
                userIng.includes(optIngredient) ||
                optIngredient.includes(userIng)
            )
          ) {
            currentMatchCount++;
            availableOptional.push(optIngredient);
          }
        });
        if (recipe.name === "Meyveli Yoğurt Kasesi") {
          const hasFruit = ingredientsArray.some((ing) =>
            [
              "muz",
              "çilek",
              "elma",
              "böğürtlen",
              "şeftali",
              "kayısı",
              "üzüm",
              "armut",
            ].some((fruit) => ing.includes(fruit))
          );
          if (
            !hasFruit &&
            !recipe.optional.some(
              (opt) =>
                ingredientsArray.includes(opt) &&
                ["muz", "çilek", "elma", "böğürtlen"].includes(opt)
            )
          )
            continue;
        }
        if (currentMatchCount > maxMatchCount) {
          maxMatchCount = currentMatchCount;
          bestMatch = {
            ...recipe,
            availableOptional,
            matchScore: currentMatchCount,
          };
        }
      }
    }

    if (bestMatch) {
      setGeneratedRecipe(bestMatch);
    } else {
      setGeneratedRecipe({
        name: "Uygun Tarif Bulunamadı",
        instructions:
          "Girdiğiniz malzemelerle eşleşen bir tarifimiz şu an için yok. Lütfen malzeme listenizi kontrol edin veya daha fazla malzeme girmeyi deneyin.",
        image:
          "https://images.unsplash.com/photo-1543339308-43e59d6b73a6?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8NXx8cXVlc3Rpb24lMjBtYXJrJTIwZm9vZHxlbnwwfHwwfHx8MA%3D%3D&auto=format&fit=crop&w=500&q=60",
        icon: "fa-question-circle",
      });
    }
    unlockAchievement("firstRecipeFound");
  };

  const genderSpecificButtonBase =
    gender === "female" ? styles.buttonFemale : styles.buttonMale;
  const genderSpecificButtonHover =
    gender === "female" ? styles.buttonFemaleHover : styles.buttonMaleHover;
  const currentCalcButtonStyle = {
    ...styles.buttonBase,
    ...(isCalcButtonHovered
      ? genderSpecificButtonHover
      : genderSpecificButtonBase),
  };
  const cardBorderStyle = {
    ...styles.card,
    borderTopColor:
      gender === "female"
        ? styles.buttonFemale.backgroundColor
        : styles.buttonMale.backgroundColor,
  };
  const workoutTitleIconColor =
    gender === "female"
      ? styles.buttonFemale.backgroundColor
      : styles.buttonMale.backgroundColor;
  const workoutSuggestionIconColor =
    gender === "female"
      ? styles.buttonFemale.backgroundColor
      : styles.buttonMale.backgroundColor;
  const favoriteActivityObjects = favoriteActivities
    .map((name) => activities.find((act) => act.name === name))
    .filter(Boolean);
  const activityOptions = [
    { value: "sedentary", label: "Hareketsiz Bir Gün (az/hiç egzersiz)" },
    { value: "light", label: "Hafif Aktif (haftada 1-3 gün egzersiz)" },
    {
      value: "moderate",
      label: "Orta Derecede Aktif (haftada 3-5 gün egzersiz)",
    },
    { value: "active", label: "Çok Aktif (haftada 6-7 gün egzersiz)" },
    { value: "veryActive", label: "Ekstra Aktif (ağır/profesyonel egzersiz)" },
  ];
  const unlockedAchievementDetails = achievements
    .map((id) => ALL_ACHIEVEMENTS[id])
    .filter(Boolean);
  const sortedWeightHistoryForList = [...weightHistory].sort((a, b) => {
    const dateA = new Date(String(a.date).split(".").reverse().join("-"));
    const dateB = new Date(String(b.date).split(".").reverse().join("-"));
    if (isNaN(dateA.getTime()) || isNaN(dateB.getTime())) return 0;
    return dateB - dateA;
  });

  const currentMenuButtonStyle = {
    ...styles.menuButton,
    ...(isMenuButtonHovered ? styles.menuButtonHover : {}),
  };
  const currentRecipeButtonStyle = {
    ...styles.recipeButton,
    ...(isRecipeButtonHovered ? styles.recipeButtonHover : {}),
  };

  return (
    <div style={cardBorderStyle}>
      {lastCalcData && (
        <div style={styles.lastCalcInfoSection}>
          <i
            className="fas fa-history"
            style={{ marginRight: "8px", color: "#87CEEB" }}
          ></i>
          <strong>En Son Hesaplama:</strong> {lastCalcData.date} -{" "}
          {lastCalcData.calories} kcal (Kilo: {lastCalcData.weight}kg, Aktivite:{" "}
          {activityOptions.find((opt) => opt.value === lastCalcData.activity)
            ?.label || lastCalcData.activity}
          )
        </div>
      )}
      <h2 style={styles.cardTitle}>
        {gender === "female" ? "Kadınlar İçin" : "Erkekler İçin"} Kalori
        Sihirbazı
      </h2>
      <form style={styles.form} onSubmit={handleCalculate}>
        <input
          style={styles.input}
          type="number"
          name="age"
          placeholder="Yaşın Kaç?"
          value={inputs.age}
          onChange={handleChange}
          min={10}
          max={100}
          required
        />
        <input
          style={styles.input}
          type="number"
          name="weight"
          placeholder="Kilon (kg)"
          value={inputs.weight}
          onChange={handleChange}
          min={30}
          max={200}
          step="0.1"
          required
        />
        <input
          style={{ ...styles.input, ...styles.inputFull }}
          type="number"
          name="height"
          placeholder="Boyun (cm)"
          value={inputs.height}
          onChange={handleChange}
          min={120}
          max={220}
          required
        />
        <select
          style={styles.select}
          name="activity"
          value={inputs.activity}
          onChange={handleChange}
        >
          {activityOptions.map((option) => (
            <option key={option.value} value={option.value}>
              {option.label}
            </option>
          ))}
        </select>
        <button
          style={currentCalcButtonStyle}
          type="submit"
          onMouseOver={() => setIsCalcButtonHovered(true)}
          onMouseOut={() => setIsCalcButtonHovered(false)}
        >
          Enerjini Hesapla!
        </button>
      </form>
      {result !== null && (
        <p style={styles.resultText}>
          Günlük alman gereken kalori miktarı yaklaşık:{" "}
          <strong>{result} kcal</strong>
        </p>
      )}
      {result !== null && (
        <div style={styles.dailyGoalSection}>
          <label style={styles.goalCheckboxLabel}>
            <input
              type="checkbox"
              style={styles.goalCheckbox}
              checked={dailyGoalCompleted}
              onChange={handleGoalChange}
            />
            Bugünkü {result} kcal hedefime ulaştım! 🎉
          </label>
        </div>
      )}

      <div style={styles.waterTrackingSection}>
        <h3 style={styles.waterTrackingTitle}>
          <i className="fas fa-tint" style={{ marginRight: "10px" }}></i>
          Günlük Su Tüketimin
        </h3>
        <div style={styles.waterDisplay}>
          İçilen: <strong>{waterConsumed}</strong> / {waterGoal} {waterUnit}
        </div>
        <div style={styles.waterVisual}>
          {Array.from({ length: waterGoal }).map((_, index) => (
            <i
              key={`waterglass-${index}`}
              className={`fas fa-glass-water`}
              style={
                index < waterConsumed
                  ? styles.waterGlassIconFilled
                  : styles.waterGlassIconEmpty
              }
              title={`${index + 1}. bardak`}
            ></i>
          ))}
        </div>
        <div style={styles.waterControls}>
          <button
            style={
              isWaterAddHovered
                ? {
                    ...styles.waterButton,
                    ...styles.waterAddButton,
                    ...styles.waterAddButtonHover,
                  }
                : { ...styles.waterButton, ...styles.waterAddButton }
            }
            onClick={handleAddWater}
            onMouseEnter={() => setIsWaterAddHovered(true)}
            onMouseLeave={() => setIsWaterAddHovered(false)}
            disabled={waterConsumed >= waterGoal}
          >
            <i className="fas fa-plus" style={{ marginRight: "5px" }}></i>
            Bardak Ekle
          </button>
          <button
            style={
              isWaterRemoveHovered
                ? {
                    ...styles.waterButton,
                    ...styles.waterRemoveButton,
                    ...styles.waterRemoveButtonHover,
                  }
                : { ...styles.waterButton, ...styles.waterRemoveButton }
            }
            onClick={handleRemoveWater}
            onMouseEnter={() => setIsWaterRemoveHovered(true)}
            onMouseLeave={() => setIsWaterRemoveHovered(false)}
            disabled={waterConsumed <= 0}
          >
            <i className="fas fa-minus" style={{ marginRight: "5px" }}></i>
            Bardak Çıkar
          </button>
        </div>
      </div>

      {/* --- YENİ: KALORİYE GÖRE MENÜ OLUŞTURUCU (FAZ 9) --- */}
      <div style={styles.menuGeneratorSection}>
        <h3 style={styles.menuGeneratorTitle}>
          <i className="fas fa-utensils" style={{ marginRight: "10px" }}></i>
          Kaloriye Göre Menü Planlayıcı
        </h3>
        <input
          type="number"
          style={styles.menuInput}
          placeholder="Hedef günlük kalori (örn: 2000)"
          value={targetMenuCalories}
          onChange={(e) => setTargetMenuCalories(e.target.value)}
          min="500"
          max="10000"
        />
        <button
          style={currentMenuButtonStyle}
          onClick={handleGenerateMenu}
          onMouseEnter={() => setIsMenuButtonHovered(true)}
          onMouseLeave={() => setIsMenuButtonHovered(false)}
        >
          <i className="fas fa-magic" style={{ marginRight: "8px" }}></i>
          Menü Oluştur
        </button>
        {generatedMenu && (
          <div style={styles.menuResultCard}>
            {generatedMenu.breakfast && (
              <p style={styles.menuMealItem}>
                <i
                  className={`fas ${generatedMenu.breakfast.icon}`}
                  style={styles.menuMealIcon}
                ></i>
                <span style={styles.menuMealName}>Kahvaltı:</span>{" "}
                {generatedMenu.breakfast.name}
                <span style={styles.menuMealCalories}>
                  {" "}
                  (~{generatedMenu.breakfast.calories} kcal)
                </span>
              </p>
            )}
            {generatedMenu.lunch && (
              <p style={styles.menuMealItem}>
                <i
                  className={`fas ${generatedMenu.lunch.icon}`}
                  style={styles.menuMealIcon}
                ></i>
                <span style={styles.menuMealName}>Öğle Yemeği:</span>{" "}
                {generatedMenu.lunch.name}
                <span style={styles.menuMealCalories}>
                  {" "}
                  (~{generatedMenu.lunch.calories} kcal)
                </span>
              </p>
            )}
            {generatedMenu.dinner && (
              <p style={styles.menuMealItem}>
                <i
                  className={`fas ${generatedMenu.dinner.icon}`}
                  style={styles.menuMealIcon}
                ></i>
                <span style={styles.menuMealName}>Akşam Yemeği:</span>{" "}
                {generatedMenu.dinner.name}
                <span style={styles.menuMealCalories}>
                  {" "}
                  (~{generatedMenu.dinner.calories} kcal)
                </span>
              </p>
            )}
            {generatedMenu.snacks.length > 0 && (
              <div style={{ marginTop: "10px" }}>
                <p style={{ ...styles.menuMealName, marginBottom: "5px" }}>
                  Ara Öğünler:
                </p>
                {generatedMenu.snacks.map((snack, index) => (
                  <p
                    key={index}
                    style={{ ...styles.menuMealItem, paddingLeft: "15px" }}
                  >
                    <i
                      className={`fas ${snack.icon}`}
                      style={styles.menuMealIcon}
                    ></i>
                    {snack.name}
                    <span style={styles.menuMealCalories}>
                      {" "}
                      (~{snack.calories} kcal)
                    </span>
                  </p>
                ))}
              </div>
            )}
            <p style={styles.menuTotalCalories}>
              Toplam Yaklaşık: {generatedMenu.totalCalories} kcal
            </p>
            <p style={styles.menuDisclaimer}>
              *Bu menü genel bir örnektir ve kalori değerleri yaklaşıktır.
              Kişisel ihtiyaçlarınız için bir diyetisyene danışmanız önerilir.
            </p>
          </div>
        )}
      </div>

      {/* --- YENİ: MALZEMELERLE TARİF BULUCU (FAZ 9) --- */}
      <div style={styles.recipeFinderSection}>
        <h3 style={styles.recipeFinderTitle}>
          <i className="fas fa-search" style={{ marginRight: "10px" }}></i>
          Mutfaktaki Malzemelerinle Ne Yapabilirsin?
        </h3>
        <textarea
          style={styles.ingredientsTextarea}
          placeholder="Elinizdeki malzemeleri virgülle ayırarak yazın (örn: tavuk, pirinç, soğan)"
          value={userIngredients}
          onChange={(e) => setUserIngredients(e.target.value)}
          rows={3}
        />
        <button
          style={currentRecipeButtonStyle}
          onClick={handleGenerateRecipe}
          onMouseEnter={() => setIsRecipeButtonHovered(true)}
          onMouseLeave={() => setIsRecipeButtonHovered(false)}
        >
          <i className="fas fa-lightbulb" style={{ marginRight: "8px" }}></i>
          Tarif Bul
        </button>
        {generatedRecipe && (
          <div style={styles.recipeResultCard}>
            <h4 style={styles.recipeName}>
              <i
                className={`fas ${generatedRecipe.icon || "fa-utensils"}`}
                style={{ marginRight: "8px" }}
              ></i>
              {generatedRecipe.name}
            </h4>
            {generatedRecipe.image && (
              <img
                src={generatedRecipe.image}
                alt={generatedRecipe.name}
                style={styles.recipeImage}
              />
            )}
            {generatedRecipe.required &&
              generatedRecipe.required.length > 0 && (
                <>
                  <p style={styles.recipeSectionTitle}>Temel Malzemeler:</p>
                  <ul style={styles.recipeIngredientsList}>
                    {generatedRecipe.required.map((ing, i) => (
                      <li key={`req-${i}`}>{ing}</li>
                    ))}
                  </ul>
                </>
              )}
            {generatedRecipe.availableOptional &&
              generatedRecipe.availableOptional.length > 0 && (
                <>
                  <p style={styles.recipeSectionTitle}>
                    Kullanabileceğin Ek Malzemeler:
                  </p>
                  <ul style={styles.recipeIngredientsList}>
                    {generatedRecipe.availableOptional.map((ing, i) => (
                      <li key={`opt-${i}`}>{ing}</li>
                    ))}
                  </ul>
                </>
              )}
            <p style={styles.recipeSectionTitle}>Hazırlanışı:</p>
            <p style={styles.recipeInstructions}>
              {generatedRecipe.instructions}
            </p>
          </div>
        )}
      </div>

      <div style={styles.achievementsSection}>
        <h3 style={styles.achievementsTitle}>
          <i className="fas fa-award" style={{ marginRight: "10px" }}></i>
          Başarıların ve Serilerin
        </h3>
        {dailyGoalStreak > 0 && (
          <p style={styles.streakInfo}>
            <i
              className="fas fa-fire-alt"
              style={{ marginRight: "8px", color: "#FF6347" }}
            ></i>
            Günlük Hedef Serin: <strong>{dailyGoalStreak} gün!</strong> Harika
            gidiyorsun!
          </p>
        )}
        {unlockedAchievementDetails.length > 0 ? (
          <ul style={styles.achievementList}>
            {unlockedAchievementDetails.map((ach) => (
              <li
                key={ach.id}
                style={styles.achievementItem}
                title={ach.description}
                onMouseEnter={(e) =>
                  (e.currentTarget.style.transform =
                    styles.achievementItemHover.transform)
                }
                onMouseLeave={(e) =>
                  (e.currentTarget.style.transform = "scale(1)")
                }
              >
                <i
                  className={`fas ${ach.icon}`}
                  style={styles.achievementIcon}
                ></i>
                <div style={styles.achievementDetails}>
                  <p style={styles.achievementName}>{ach.name}</p>
                  <p style={styles.achievementDescription}>{ach.description}</p>
                </div>
              </li>
            ))}
          </ul>
        ) : (
          <p style={styles.noAchievementsText}>
            Henüz bir başarı kazanmadın. Siteyi keşfetmeye devam et!
          </p>
        )}
      </div>

      <div style={styles.weightTrackingSection}>
        <h3 style={styles.weightTrackingTitle}>
          <i
            className="fas fa-weight-scale"
            style={{ marginRight: "10px" }}
          ></i>
          Kilo Takip Geçmişin
        </h3>
        <div style={styles.weightInputForm}>
          <input
            type="number"
            style={styles.weightInput}
            placeholder="Güncel kilonu gir (kg)"
            value={currentWeightInput}
            onChange={handleCurrentWeightInputChange}
            min="20"
            step="0.1"
          />
          <button
            style={
              isWeightAddButtonHovered
                ? { ...styles.weightAddButton, ...styles.weightAddButtonHover }
                : styles.weightAddButton
            }
            onClick={handleAddWeightEntry}
            onMouseEnter={() => setIsWeightAddButtonHovered(true)}
            onMouseLeave={() => setIsWeightAddButtonHovered(false)}
          >
            <i className="fas fa-plus" style={{ marginRight: "5px" }}></i> Ekle
          </button>
        </div>
        <WeightChart weightHistory={weightHistory} gender={gender} />
        {sortedWeightHistoryForList.length > 0 ? (
          <ul style={styles.weightHistoryList}>
            {sortedWeightHistoryForList.map((entry, index) => (
              <li
                key={`${entry.date}-${entry.weight}-${index}`}
                style={styles.weightHistoryItem}
              >
                <div>
                  <span style={styles.weightHistoryDate}>{entry.date}</span> -{" "}
                  <span style={styles.weightHistoryValue}>
                    {entry.weight.toFixed(1)} kg
                  </span>
                </div>
                <button
                  style={styles.weightDeleteButton}
                  onClick={() => handleDeleteWeightEntry(entry)}
                  title="Bu kaydı sil"
                  onMouseEnter={(e) =>
                    (e.currentTarget.style.color =
                      styles.weightDeleteButtonHover.color)
                  }
                  onMouseLeave={(e) =>
                    (e.currentTarget.style.color =
                      styles.weightDeleteButton.color)
                  }
                >
                  <i className="fas fa-trash-alt"></i>
                </button>
              </li>
            ))}
          </ul>
        ) : (
          <p style={styles.noWeightHistoryText}>
            Henüz kilo kaydın bulunmuyor. İlk kaydını ekleyerek takibe başla!
          </p>
        )}
      </div>

      {nutritionInfo && (
        <div style={styles.nutritionGuidanceSection}>
          <h3 style={styles.nutritionTitle}>
            <i className="fas fa-utensils" style={{ marginRight: "10px" }}></i>
            {nutritionInfo.title}
          </h3>
          {nutritionInfo.points.map((point, index) => (
            <p key={index} style={styles.nutritionText}>
              <i
                className="fas fa-check-circle"
                style={{ marginRight: "8px", color: "#66CDAA" }}
              ></i>
              {point}
            </p>
          ))}
          <p style={styles.nutritionDisclaimer}>{nutritionInfo.disclaimer}</p>
        </div>
      )}
      {result !== null && workoutPlan && (
        <div style={styles.workoutSection}>
          <h3 style={styles.workoutTitle}>
            <i
              className={`fas ${workoutPlan.icon}`}
              style={{ marginRight: "10px", color: workoutTitleIconColor }}
            ></i>
            Sana Özel Aktivite Önerisi
          </h3>
          <ul style={styles.workoutSuggestionList}>
            <li style={styles.workoutSuggestionItem}>
              <i
                className={`fas ${workoutPlan.icon}`}
                style={{
                  ...styles.workoutSuggestionIcon,
                  color: workoutSuggestionIconColor,
                }}
              ></i>
              {workoutPlan.name}
            </li>
          </ul>
          <p style={styles.workoutNote}>
            Bu sadece bir öneri! Kendine en uygun aktiviteyi seçebilirsin.
            Önemli olan hareket etmek!
          </p>
        </div>
      )}
      {result !== null && inputs.weight && (
        <div style={styles.plannedWorkoutSection}>
          <h3 style={styles.plannedWorkoutTitle}>
            <i className="fas fa-running" style={{ marginRight: "10px" }}></i>
            Planladığın Egzersiz Ne Kadar Yakar?
          </h3>
          <div style={styles.plannedWorkoutForm}>
            <select
              style={{ ...styles.select, gridColumn: "auto" }}
              name="type"
              value={plannedWorkout.type}
              onChange={handlePlannedWorkoutChange}
            >
              {activities.map((act) => (
                <option key={act.name} value={act.name}>
                  {act.name}
                </option>
              ))}
            </select>
            <input
              style={{ ...styles.input, gridColumn: "auto" }}
              type="number"
              name="duration"
              placeholder="Süre (dakika)"
              value={plannedWorkout.duration}
              onChange={handlePlannedWorkoutChange}
              min={10}
            />
            <button
              style={{
                ...styles.buttonBase,
                gridColumn: "auto",
                marginTop: "0",
                padding: "16px",
                backgroundColor: "#5DADE2",
                fontSize: "16px",
              }}
              onClick={handleEstimateWorkoutBurn}
              onMouseOver={(e) =>
                (e.currentTarget.style.backgroundColor = "#3498DB")
              }
              onMouseOut={(e) =>
                (e.currentTarget.style.backgroundColor = "#5DADE2")
              }
            >
              Tahmin Et
            </button>
          </div>
          {estimatedWorkoutBurn !== null && (
            <p style={styles.plannedWorkoutResult}>
              Bu egzersizle yaklaşık{" "}
              <strong>{estimatedWorkoutBurn} kcal</strong> yakabilirsin! 🔥
            </p>
          )}
        </div>
      )}
      {result !== null && (
        <div style={styles.locationActionSection}>
          <h3 style={styles.locationActionTitle}>
            <i
              className="fas fa-map-marker-alt"
              style={{ marginRight: "10px" }}
            ></i>
            Harekete Geçmek İçin Yakın Yerler Bul!
          </h3>
          <p style={styles.locationActionSubtitle}>
            Konumunu paylaşarak sana en yakın parkları, spor salonlarını veya
            yüzme havuzlarını keşfet.
          </p>
          <div style={styles.locationButtonsContainer}>
            {["park", "gym", "pool"].map((type) => {
              const baseStyle =
                type === "park"
                  ? styles.locationButtonPark
                  : type === "gym"
                  ? styles.locationButtonGym
                  : styles.locationButtonPool;
              const hoverStyle =
                type === "park"
                  ? styles.locationButtonParkHover
                  : type === "gym"
                  ? styles.locationButtonGymHover
                  : styles.locationButtonPoolHover;
              const icon =
                type === "park"
                  ? "fa-tree"
                  : type === "gym"
                  ? "fa-dumbbell"
                  : "fa-person-swimming";
              const text =
                type === "park"
                  ? "Yakın Parklar"
                  : type === "gym"
                  ? "Yakın Spor Salonları"
                  : "Yakın Havuzlar";
              return (
                <button
                  key={type}
                  style={{
                    ...styles.locationButton,
                    ...(hoveredLocationButton === type
                      ? hoverStyle
                      : baseStyle),
                  }}
                  onClick={() => handleLocationRequest(type)}
                  onMouseEnter={() => setHoveredLocationButton(type)}
                  onMouseLeave={() => setHoveredLocationButton("")}
                >
                  <i
                    className={`fas ${icon}`}
                    style={{ marginRight: "8px" }}
                  ></i>
                  {text}
                </button>
              );
            })}
          </div>
          {locationStatus === "loading" && (
            <p style={styles.locationStatusText}>
              Konumun alınıyor, lütfen bekle...
            </p>
          )}
          {locationStatus === "error" && (
            <p style={styles.locationStatusText}>
              Konum alınamadı. Lütfen tarayıcı izinlerini kontrol et.
            </p>
          )}
          {locationStatus === "unsupported" && (
            <p style={styles.locationStatusText}>
              Tarayıcın konum servisini desteklemiyor.
            </p>
          )}
          {locationStatus === "success" && mapSearchQuery && (
            <a
              href={mapSearchQuery}
              target="_blank"
              rel="noopener noreferrer"
              style={
                hoveredLocationButton === "mapLink"
                  ? { ...styles.mapLinkButton, ...styles.mapLinkButtonHover }
                  : styles.mapLinkButton
              }
              onMouseEnter={() => setHoveredLocationButton("mapLink")}
              onMouseLeave={() => setHoveredLocationButton("")}
            >
              <i
                className="fas fa-map-marked-alt"
                style={{ marginRight: "8px" }}
              ></i>
              Haritada Göster
            </a>
          )}
        </div>
      )}
      {result !== null && (
        <div style={styles.favoriteSection}>
          <h3 style={styles.favoriteTitle}>
            <i className="fas fa-heart" style={{ marginRight: "10px" }}></i>
            Favori Aktivitelerin
          </h3>
          {favoriteActivityObjects.length > 0 ? (
            <div>
              {favoriteActivityObjects.map((act) => (
                <ActivityRow
                  key={`fav-${act.name}`}
                  activity={act}
                  gender={gender}
                  isFavorite={true}
                  onToggleFavorite={toggleFavoriteActivity}
                />
              ))}
            </div>
          ) : (
            <p style={styles.noFavoritesText}>
              Henüz favori aktiviten yok. Listeden yıldızlara tıklayarak
              ekleyebilirsin!
            </p>
          )}
        </div>
      )}
      {result !== null && (
        <div style={{ marginTop: "30px" }}>
          <h3 style={styles.activitiesTitle}>
            <i
              className="fas fa-fire"
              style={{ marginRight: "8px", color: "#FF7F50" }}
            ></i>
            Keyif Alacağın Diğer Aktiviteler
          </h3>
          <div>
            {activities.map((act) => (
              <ActivityRow
                key={act.name}
                activity={act}
                gender={gender}
                isFavorite={favoriteActivities.includes(act.name)}
                onToggleFavorite={toggleFavoriteActivity}
              />
            ))}
          </div>
        </div>
      )}
    </div>
  );
}

export default function App() {
  useEffect(() => {
    const fontLink = document.createElement("link");
    fontLink.href =
      "https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600;700&display=swap";
    fontLink.rel = "stylesheet";
    document.head.appendChild(fontLink);

    const faLink = document.createElement("link");
    faLink.rel = "stylesheet";
    faLink.href =
      "https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css";
    document.head.appendChild(faLink);

    let chartJsScript = document.getElementById("chartjs-script");
    if (!chartJsScript) {
      chartJsScript = document.createElement("script");
      chartJsScript.id = "chartjs-script";
      chartJsScript.src = "https://cdn.jsdelivr.net/npm/chart.js";
      chartJsScript.async = true;
      document.body.appendChild(chartJsScript);
    }

    return () => {
      if (document.head.contains(fontLink)) document.head.removeChild(fontLink);
      if (document.head.contains(faLink)) document.head.removeChild(faLink);
    };
  }, []);

  const healthTips = [
    "💧 Bol su içmeyi ihmal etme, hidrasyon çok önemli!",
    "😴 Yeterli ve kaliteli uyku, enerjinin anahtarıdır.",
    "🧘‍♀️ Stres yönetimi için kendine zaman ayır, meditasyon veya hobilerinle rahatla.",
    "🍏 Dengeli ve çeşitli beslenmeye özen göster, her renkten sebze ve meyve tüket.",
    "🚶‍♂️ Gün içinde aktif olmaya çalış, küçük molalarda bile hareket et.",
  ];
  const [currentTip, setCurrentTip] = useState(healthTips[0]);
  useEffect(() => {
    const tipInterval = setInterval(() => {
      setCurrentTip(healthTips[Math.floor(Math.random() * healthTips.length)]);
    }, 10000);
    return () => clearInterval(tipInterval);
  }, []);

  return (
    <div style={styles.appContainer}>
      <header style={styles.header}>
        <h1 style={styles.h1}>
          Kişisel Kalori ve Aktivite Yolculuğun Başlıyor!
        </h1>
        <p style={styles.p}>
          Sağlıklı bir sen için ilk adımı at: Günlük ihtiyaçlarını öğren, sana
          özel önerilerle harekete geç!
        </p>
      </header>
      <main style={styles.main}>
        <CalorieCalculator gender="female" />
        <CalorieCalculator gender="male" />
      </main>
      <footer style={styles.footer}>
        &copy; {new Date().getFullYear()} Bu sitenin tüm hakları saklıdır.
        Sağlıkla ve enerjiyle kal!
        <div style={styles.footerTips}>
          <strong>Minik Bir Hatırlatma:</strong> {currentTip}
        </div>
      </footer>
    </div>
  );
}
