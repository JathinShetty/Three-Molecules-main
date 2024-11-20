

# Molecule Visualization in AR (3D Molecular Modeling)

This project allows users to interact with and create 3D molecular models in an Augmented Reality (AR) environment. It utilizes **Three.js** for rendering 3D objects, **WebXR** for AR support, and **JavaScript** for interactivity. The goal of this project is to visualize molecules using atomic structures and visualize electron configurations and bonds in real-time. Users can also interact with periodic table elements in AR and create valid molecules based on atom combinations.

---

## Features

- **Interactive Periodic Table**: Visualize the periodic table elements as clickable buttons in the AR environment. Each element, when clicked, generates a 3D atom model.
- **Create Molecules**: Combine atoms to create valid molecules based on predefined patterns. Molecules are formed when users select atoms, and the system checks if the atom combination is valid.
- **Electron Visualization**: Each atom displays valence electrons around it, and electrons are dynamically generated based on the atom's properties.
- **Molecular Bonds**: Bonds are created between atoms when valid molecules are formed. The bonds dynamically adjust based on atom selection.
- **AR Mode**: View the periodic table in AR and interact with atoms using **WebXR** for immersive experiences.
  
---

## Setup

To run this project, ensure you have the following installed:

- **Web Browser** with WebXR support (e.g., Chrome or Firefox)
- **Three.js**: A 3D JavaScript library used for rendering 3D graphics.
- **CSS2DRenderer**: For rendering HTML elements as labels in 3D space.
- **WebXR API**: For enabling Augmented Reality features.

### Installation

1. **Clone the Repository:**

```bash
git clone <repository_url>
```

2. **Include Three.js and CSS2DRenderer**

The project includes **Three.js** for 3D rendering and **CSS2DRenderer** for handling 2D HTML labels in 3D space. Ensure these libraries are loaded in your HTML file:

```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
<script src="https://threejs.org/examples/js/renderers/CSS2DRenderer.js"></script>
```

3. **Dependencies**

No external dependencies are needed beyond the Three.js library and AR-related API support in the browser.

---

## Usage

### Initial Setup

1. Open the `index.html` file in a browser with **WebXR** support.
2. The periodic table of elements is rendered in the AR space.
3. Click on elements to create atoms, which are placed in the 3D scene.
4. Combine atoms to form molecules and visualize bonds.
5. AR functionality allows viewing in immersive mode on supported devices.

### Periodic Table AR Mode

When AR mode is activated:

- The periodic table is displayed as a set of interactive buttons.
- Each element in the periodic table is represented by a 3D plane, and users can click on an element to generate an atom in the 3D space.
- Users can interact with atoms and form molecules by clicking on atoms and validating the molecule combinations.

### Creating Molecules

- Atoms are selected from the periodic table and added to the `selectedAtoms` array.
- The function `createMolecule` checks if the combination of atoms is a valid molecule.
- Once a valid compound is identified, atoms are arranged in 3D space, and electrons are created for each atom.

### Electron Visualization

- For each atom, electrons are dynamically visualized based on its valence electrons (from the periodic table data).
- Electrons are represented as small spheres placed around each atom.

### Bonds

- Bonds are created between atoms once they are selected and validated as part of a molecule.
- The `createBond` function creates the bond between two atoms and adds it to the scene.

---

## Code Explanation

### Periodic Table Button Rendering in AR

The `renderPeriodicTableButtonsAR()` function creates clickable buttons for each element in the periodic table and displays them in 3D space. When a user clicks on an element, it creates an atom and adds it to the `selectedAtoms` array.

```javascript
function renderPeriodicTableButtonsAR() {
    // Create a group for periodic table elements
    const periodicTableGroup = new THREE.Group();
    const elements = ['H', 'He', 'Li', 'Be', 'B', 'C', 'N', 'O', 'F', 'Ne'];
    
    // Define positions and create buttons for each element
    elements.forEach((element, index) => {
        const button = createButton(element, index);
        periodicTableGroup.add(button);
    });

    scene.add(periodicTableGroup);
}
```

### Creating a Molecule

The `createMolecule()` function checks if the selected atoms form a valid compound based on predefined data. It rearranges atoms in 3D space and adds electron visualizations.

```javascript
function createMolecule() {
    const compound = findValidCompound(selectedAtoms);
    
    if (!compound) {
        showMessage('Invalid combination of atoms!', true);
        return;
    }
    
    bonds.forEach(bond => scene.remove(bond));
    bonds = [];
    
    // Reposition atoms and create electrons
    selectedAtoms.forEach((atom, index) => {
        atom.position.copy(compound.positions[index]);
        recreateElectrons(atom);
    });

    // Create bonds between atoms
    compound.bonds.forEach(bondInfo => {
        createBond(selectedAtoms[bondInfo.from], selectedAtoms[bondInfo.to]);
    });
}
```

### AR Session Handling

The AR session is initiated using the WebXR API. When the user clicks the AR button, the scene switches to AR mode.

```javascript
document.getElementById('arButton').addEventListener('click', () => {
    if (navigator.xr) {
        navigator.xr.requestSession('immersive-ar', {
            requiredFeatures: ['hit-test']
        }).then((session) => {
            console.log("AR session started");
            xrSession = session;
            renderer.xr.enabled = true;
            renderer.xr.setReferenceSpaceType('local');
            renderer.xr.setSession(session);
            renderPeriodicTableAR();
            animateAR();
        }).catch((error) => {
            console.error("Error starting AR session", error);
        });
    } else {
        alert("AR not supported on this device.");
    }
});
```

---

## License

This project is open-source and available under the [MIT License](LICENSE).

---

## Contributions

Feel free to fork the repository, create issues, and submit pull requests for any enhancements or bug fixes.

---

## Troubleshooting

- **WebXR not supported**: Ensure your browser supports WebXR. Chrome and Firefox are recommended for AR experiences.
- **Molecule creation fails**: Ensure you're selecting atoms that form a valid compound. Review the available compound patterns in the `validCompounds` object.
- **Electron visualization issue**: If electrons aren't showing correctly, ensure the `valenceElectrons` data for each atom is populated correctly.
