# ironhack-lab-3
Repositorio de Laboratorio 3 de IronHack

# Lab: Effective Test Writing Techniques

## Lab Overview

This lab aims to sharpen your abilities in applying effective test writing techniques. You’ll focus on enhancing clarity, simplicity, and robustness in test cases. By evaluating and refining provided test scenarios, you’ll practice optimizing test effectiveness using principles from your previous training.

### Test Case Analysis and Refinement

**Tasks:**
1. **Analysis:** Review the provided test cases that simulate common functionalities but contain deliberate flaws or inefficiencies.
2. **Refinement:** Improve these test cases using both theoretical insights and practical applications from previous lessons.

## Scenarios for Test Case Analysis

### Scenario 1: User Authentication Tests

**Original Test Case (Pseudocode):**
```pseudocode
TEST UserAuthentication
  ASSERT_TRUE(authenticate("validUser", "validPass"), "Should succeed with correct credentials")
  ASSERT_FALSE(authenticate("validUser", "wrongPass"), "Should fail with wrong credentials")
END TEST
```

#### Analysis Report
- **Claridad:** El nombre del test "UserAuthentication" es muy "amplio" en el sentido que puede significar distintos casos en vez de indicar un caso en especifico.
- **Single Responsibility:** El test trata de probar 2 diferencias escenarios (credenciales validas e invalidas) en un mismo test.
- **Isolación:** El test combina multiples assertions en un solo test lo que hace dificil de identificar problemas en especificos.

#### Revised Test Case
```pseudocode
TEST UserAuthenticationWithValidCredentials
  boolean result = authenticate("validUser", "validPass")
  ASSERT_TRUE(result, "Authentication should succeed with correct credentials")
END TEST

TEST UserAuthenticationWithInvalidCredentials
  boolean result = authenticate("validUser", "wrongPass")
  ASSERT_FALSE(result, "Authentication should fail with incorrect credentials")
END TEST
```

**Rationale:**
- **Claridad Mejorada:** Cada nombre de test especifica el escenario exacto a probar.
- **Single Responsibility:** Cada test ahora cumple con un escenario en especifico.
- **Isolación Mejorada:** Realizando diferentes test invidivuales, nos aseguramos que cada uno independientemente cumpla con una sola función, de tal forma que si se presenta cualquier error o falla sea mas facil de diagnosticar.

### Scenario 2: Data Processing Functions

**Original Test Case (Pseudocode):**
```pseudocode
TEST DataProcessing
  DATA data = fetchData()
  TRY
    processData(data)
    ASSERT_TRUE(data.processedSuccessfully, "Data should be processed successfully")
  CATCH error
    ASSERT_EQUALS("Data processing error", error.message, "Should handle processing errors")
  END TRY
END TEST
```

#### Issues in the Original Test Case
- **Claridad:** El nombre del test "DataProcessing" de igual forma es muy "amplio" en el sentido que puede significar distintos casos en vez de indicar un caso en especifico.
- **Single Responsibility:** El test está tratando de probar 2 casos diferentes y al mismo tiempo un exception handling.
- **Isolación:** Combinar 2 operaciones normales y una exception handling en el mismo test puede ocultar problemas resultantes o específicos.

#### Revised Test Case
```pseudocode
TEST DataProcessingSuccessful
  DATA data = fetchData()
  processData(data)
  ASSERT_TRUE(data.processedSuccessfully, "Data should be processed successfully")
END TEST

TEST DataProcessingErrorHandling
  DATA data = fetchDataWithErrors()
  TRY
    processData(data)
  CATCH error
    ASSERT_EQUALS("Data processing error", error.message, "Should handle processing errors")
  END TRY
END TEST
```

**Rationale:**
- **Claridad Mejorada:** Los nombres de los test ahora especifican los escenarios probados (successful processing and error handling).
- **Single Responsibility:** Cada test ahora se enfoca en un aspecto en específico del data processing, ya sea successful o un error handling.
- **Isolación Mejorada:** Separando los test nos aseguramos que cada uno sea independiente, al igual que los posibles errores surgidos, facilitando el diagnostico.

### Scenario 3: UI Responsiveness

**Original Test Case (Pseudocode):**
```pseudocode
TEST UIResponsiveness
  UI_COMPONENT uiComponent = setupUIComponent(1024)
  ASSERT_TRUE(uiComponent.adjustsToScreenSize(1024), "UI should adjust to width of 1024 pixels")
END TEST
```

#### Issues in the Original Test Case
- **Claridad:** El nombre del test cuenta con el mismo problema de los 2 anterioes, "UIResponsiveness" no es nada claro ni especifico hacia que aspecto intenta probar.
- **Single Responsibility:** Solamente 1 tamaño de pantalla es probado, no existe validación para otros tamaños o casos.
- **Isolación:** La falta de tests para diferentes casos de tamaños de pantalla limita la robustes y response al probar el UI.

#### Revised Test Case
```pseudocode
TEST UIResponsivenessAt1024Pixels
  UI_COMPONENT uiComponent = setupUIComponent(1024)
  boolean result = uiComponent.adjustsToScreenSize(1024)
  ASSERT_TRUE(result, "UI should adjust to width of 1024 pixels")
END TEST

TEST UIResponsivenessAtDifferentScreenSizes
  UI_COMPONENT uiComponent = setupUIComponent(800)
  boolean result800 = uiComponent.adjustsToScreenSize(800)
  ASSERT_TRUE(result800, "UI should adjust to width of 800 pixels")

  uiComponent = setupUIComponent(1280)
  boolean result1280 = uiComponent.adjustsToScreenSize(1280)
  ASSERT_TRUE(result1280, "UI should adjust to width of 1280 pixels")
END TEST
```

**Rationale:**
- **Claridad Mejorada:** Los tests ahora especifican los tamaños de pantalla a ser probados, haciendo claro la intencion de cada test.
- **Single Responsibility:** Cada prueba se enfoca en un tamaño de pantalla, asegurandonos de verficiar el comportamiento de cada caso especifico.
- **Isolación Mejorada:** Se sepatan los tests para distintos escenarios y tamaños de pantalla, facilitando el entendimiento de las pruebas y el diagnostico de errores.
