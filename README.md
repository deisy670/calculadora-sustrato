import { useState } from "react";

export default function SustratoForm() {
  const [area, setArea] = useState("1");
  const [profundidad, setProfundidad] = useState("10");
  const [humedadTierra, setHumedadTierra] = useState("seca");
  const [humedadCompost, setHumedadCompost] = useState("seca");
  const [humedadLombriabono, setHumedadLombriabono] = useState("seca");
  const [porcentajeTierra, setPorcentajeTierra] = useState(50);
  const [porcentajeCompost, setPorcentajeCompost] = useState(30);
  const [porcentajeLombriabono, setPorcentajeLombriabono] = useState(20);
  
  // Campos para valor por kg en pesos
  const [valorTierra, setValorTierra] = useState("0");
  const [valorCompost, setValorCompost] = useState("0");
  const [valorLombriabono, setValorLombriabono] = useState("0");

  const [resultado, setResultado] = useState(null);

  const densidades = {
    seca: { tierra: 1200, compost: 800, lombriabono: 600 },
    humeda: { tierra: 1400, compost: 1000, lombriabono: 800 }
  };

  const calcularSustrato = () => {
    // Convertir entradas a números
    const numArea = parseFloat(area) || 0;
    const numProfundidad = parseFloat(profundidad) || 0;
    const numValorTierra = parseFloat(valorTierra) || 0;
    const numValorCompost = parseFloat(valorCompost) || 0;
    const numValorLombriabono = parseFloat(valorLombriabono) || 0;
    
    const volumen = numArea * (numProfundidad / 100);
    const tierra = volumen * (porcentajeTierra / 100);
    const compost = volumen * (porcentajeCompost / 100);
    const lombriabono = volumen * (porcentajeLombriabono / 100);
    const pesoTierra = tierra * densidades[humedadTierra].tierra;
    const pesoCompost = compost * densidades[humedadCompost].compost;
    const pesoLombriabono = lombriabono * densidades[humedadLombriabono].lombriabono;
    const pesoTotal = pesoTierra + pesoCompost + pesoLombriabono;
    
    // Calcular costos individuales
    const costoTierra = pesoTierra * numValorTierra;
    const costoCompost = pesoCompost * numValorCompost;
    const costoLombriabono = pesoLombriabono * numValorLombriabono;
    const costoTotal = costoTierra + costoCompost + costoLombriabono;
    
    setResultado({ 
      volumen, 
      pesoTierra, 
      pesoCompost, 
      pesoLombriabono, 
      pesoTotal, 
      costoTierra, 
      costoCompost, 
      costoLombriabono, 
      costoTotal 
    });
  };

  // Función para formatear números con separador de miles y 1 decimal.
  const formatear = (num) =>
    num.toLocaleString("es-ES", {
      minimumFractionDigits: 1,
      maximumFractionDigits: 1,
    });

  return (
    <div style={{ maxWidth: "400px", margin: "20px auto", padding: "20px", border: "1px solid #ccc", borderRadius: "8px" }}>
      <h2>Calculadora de Sustrato</h2>
      
      <label>Área (m²)</label>
      <input
        type="text"
        value={area}
        onChange={(e) => setArea(e.target.value)}
        style={{ width: "100%", padding: "8px", marginBottom: "10px" }}
      />
      
      <label>Profundidad (cm)</label>
      <input
        type="text"
        value={profundidad}
        onChange={(e) => setProfundidad(e.target.value)}
        style={{ width: "100%", padding: "8px", marginBottom: "10px" }}
      />

      <label>Humedad de la Tierra</label>
      <select value={humedadTierra} onChange={(e) => setHumedadTierra(e.target.value)} style={{ width: "100%", padding: "8px", marginBottom: "10px" }}>
        <option value="seca">Seca</option>
        <option value="humeda">Húmeda</option>
      </select>

      <label>Humedad del Compost</label>
      <select value={humedadCompost} onChange={(e) => setHumedadCompost(e.target.value)} style={{ width: "100%", padding: "8px", marginBottom: "10px" }}>
        <option value="seca">Seca</option>
        <option value="humeda">Húmeda</option>
      </select>

      <label>Humedad del Lombriabono</label>
      <select value={humedadLombriabono} onChange={(e) => setHumedadLombriabono(e.target.value)} style={{ width: "100%", padding: "8px", marginBottom: "10px" }}>
        <option value="seca">Seca</option>
        <option value="humeda">Húmeda</option>
      </select>

      <label>Porcentaje de Tierra (%)</label>
      <input
        type="number"
        value={porcentajeTierra}
        onChange={(e) => setPorcentajeTierra(Math.max(0, parseFloat(e.target.value) || 0))}
        style={{ width: "100%", padding: "8px", marginBottom: "10px" }}
      />

      <label>Porcentaje de Compost (%)</label>
      <input
        type="number"
        value={porcentajeCompost}
        onChange={(e) => setPorcentajeCompost(Math.max(0, parseFloat(e.target.value) || 0))}
        style={{ width: "100%", padding: "8px", marginBottom: "10px" }}
      />

      <label>Porcentaje de Lombriabono (%)</label>
      <input
        type="number"
        value={porcentajeLombriabono}
        onChange={(e) => setPorcentajeLombriabono(Math.max(0, parseFloat(e.target.value) || 0))}
        style={{ width: "100%", padding: "8px", marginBottom: "10px" }}
      />

      <label>Valor de la Tierra (por kg en pesos)</label>
      <input
        type="text"
        value={valorTierra}
        onChange={(e) => setValorTierra(e.target.value)}
        style={{ width: "100%", padding: "8px", marginBottom: "10px" }}
      />

      <label>Valor del Compost (por kg en pesos)</label>
      <input
        type="text"
        value={valorCompost}
        onChange={(e) => setValorCompost(e.target.value)}
        style={{ width: "100%", padding: "8px", marginBottom: "10px" }}
      />

      <label>Valor del Lombriabono (por kg en pesos)</label>
      <input
        type="text"
        value={valorLombriabono}
        onChange={(e) => setValorLombriabono(e.target.value)}
        style={{ width: "100%", padding: "8px", marginBottom: "10px" }}
      />

      <button
        onClick={calcularSustrato}
        style={{ marginTop: "10px", padding: "10px", backgroundColor: "#28a745", color: "#fff", border: "none", borderRadius: "5px", cursor: "pointer", width: "100%" }}
      >
        Calcular
      </button>

      {resultado && (
        <div style={{ marginTop: "20px", padding: "10px", backgroundColor: "#f8f9fa", borderRadius: "5px" }}>
          <p><strong>Volumen:</strong> {formatear(resultado.volumen)} m³</p>
          <p><strong>Peso Tierra:</strong> {formatear(resultado.pesoTierra)} kg</p>
          <p><strong>Peso Compost:</strong> {formatear(resultado.pesoCompost)} kg</p>
          <p><strong>Peso Lombriabono:</strong> {formatear(resultado.pesoLombriabono)} kg</p>
          <p><strong>Peso Total:</strong> {formatear(resultado.pesoTotal)} kg</p>
          <p><strong>Costo Tierra:</strong> {resultado.pesoTierra > 0 ? formatear(resultado.pesoTierra * parseFloat(valorTierra || 0)) : "0"} pesos</p>
          <p><strong>Costo Compost:</strong> {resultado.pesoCompost > 0 ? formatear(resultado.pesoCompost * parseFloat(valorCompost || 0)) : "0"} pesos</p>
          <p><strong>Costo Lombriabono:</strong> {resultado.pesoLombriabono > 0 ? formatear(resultado.pesoLombriabono * parseFloat(valorLombriabono || 0)) : "0"} pesos</p>
          <p><strong>Costo Total:</strong> {formatear(resultado.costoTotal)} pesos</p>
        </div>
      )}
    </div>
  );
}
