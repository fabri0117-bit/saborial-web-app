// Proyecto: Sistema Web para SABORIAL
// Framework: React + Tailwind CSS + Simulación de autenticación por roles

import React, { useState } from 'react';
import { BrowserRouter as Router, Route, Routes, Link, Navigate } from 'react-router-dom';

const fakeUsers = [
  { username: 'admin', password: 'admin123', role: 'administrador' },
  { username: 'operario1', password: 'operario123', role: 'operario' },
  { username: 'cliente1', password: 'cliente123', role: 'cliente' },
];

const Navbar = ({ role }) => (
  <nav className="bg-gray-800 p-4 text-white flex justify-between">
    <div className="font-bold">SABORIAL</div>
    <div className="space-x-4">
      <Link to="/">Inicio</Link>
      {role && <Link to="/inventario">Inventario</Link>}
      {role && <Link to="/produccion">Producción</Link>}
      {role && <Link to="/ventas">Ventas</Link>}
      {role && <Link to="/clientes">Clientes</Link>}
      {role === 'administrador' && <Link to="/usuarios">Usuarios</Link>}
      {role && <Link to="/reportes">Reportes</Link>}
    </div>
  </nav>
);

const Home = () => (
  <div className="p-8 text-center">
    <h1 className="text-3xl font-bold mb-4">Bienvenido al sistema SABORIAL</h1>
    <p>Seleccione un módulo desde la barra de navegación.</p>
  </div>
);

const Login = ({ onLogin }) => {
  const [credentials, setCredentials] = useState({ username: '', password: '' });
  const [error, setError] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    const user = fakeUsers.find(
      (u) => u.username === credentials.username && u.password === credentials.password
    );
    if (user) {
      onLogin(user);
    } else {
      setError('Credenciales inválidas');
    }
  };

  return (
    <div className="p-6 max-w-md mx-auto">
      <h2 className="text-2xl font-bold mb-4">Iniciar Sesión</h2>
      <form onSubmit={handleSubmit} className="space-y-4">
        <input
          name="username"
          placeholder="Usuario"
          value={credentials.username}
          onChange={(e) => setCredentials({ ...credentials, username: e.target.value })}
          className="border p-2 w-full"
          required
        />
        <input
          name="password"
          type="password"
          placeholder="Contraseña"
          value={credentials.password}
          onChange={(e) => setCredentials({ ...credentials, password: e.target.value })}
          className="border p-2 w-full"
          required
        />
        <button type="submit" className="bg-blue-600 text-white px-4 py-2 rounded">
          Ingresar
        </button>
        {error && <p className="text-red-600 mt-2">{error}</p>}
      </form>
    </div>
  );
};

const ModuleTemplate = ({ title }) => (
  <div className="p-6">
    <h2 className="text-2xl font-bold mb-4">{title}</h2>
    <p>Contenido del módulo en construcción...</p>
  </div>
);

// Reutilizamos componentes de módulos como Inventario, Produccion, etc. (omitidos aquí por brevedad)

const App = () => {
  const [user, setUser] = useState(null);

  return (
    <Router>
      <Navbar role={user?.role} />
      <Routes>
        {!user && <Route path="/*" element={<Login onLogin={setUser} />} />}
        {user && (
          <>
            <Route path="/" element={<Home />} />
            <Route path="/inventario" element={<Inventario />} />
            <Route path="/produccion" element={<Produccion />} />
            <Route path="/ventas" element={<Ventas />} />
            <Route path="/clientes" element={<Clientes />} />
            {user.role === 'administrador' && <Route path="/usuarios" element={<Usuarios />} />}
            <Route path="/reportes" element={<Reportes />} />
            <Route path="/*" element={<Navigate to="/" />} />
          </>
        )}
      </Routes>
    </Router>
  );
};

export default App;

