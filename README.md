# Busca-de-Perfil-no-GitHubimport React, { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Input } from "@/components/ui/input";
import { Loader2, Search } from "lucide-react";

const GitHubProfileSearch = () => {
  const [username, setUsername] = useState("");
  const [userData, setUserData] = useState(null);
  const [error, setError] = useState(null);
  const [loading, setLoading] = useState(false);

  const fetchGitHubProfile = async () => {
    if (!username) return;
    setLoading(true);
    setError(null);
    setUserData(null);

    try {
      const response = await fetch(`https://api.github.com/users/${username}`);
      if (!response.ok) {
        throw new Error("Nenhum perfil foi encontrado com esse nome de usuário.\nTente novamente.");
      }
      const data = await response.json();
      setUserData(data);
    } catch (err) {
      setError(err.message);
    } finally {
      setLoading(false);
    }
  };

  return (
    <div className="flex flex-col items-center justify-center min-h-screen bg-black text-white p-4">
      <div className="w-full max-w-lg p-6 bg-black shadow-lg rounded-lg text-center border border-gray-700">
        <h1 className="text-3xl font-bold flex items-center justify-center space-x-2">
          <span className="text-white">Perfil</span>
          <span className="text-blue-500">GitHub</span>
        </h1>
        <div className="flex items-center mt-6 bg-white rounded-lg overflow-hidden">
          <Input
            type="text"
            placeholder="Digite um usuário do GitHub"
            className="flex-grow px-4 py-2 text-black"
            value={username}
            onChange={(e) => setUsername(e.target.value)}
          />
          <Button onClick={fetchGitHubProfile} disabled={loading} className="bg-blue-500 px-4 py-2">
            {loading ? <Loader2 className="animate-spin" /> : <Search />}
          </Button>
        </div>
        {error && <p className="text-red-500 mt-4 bg-gray-200 text-center p-2 rounded-md">{error}</p>}
        {userData && (
          <Card className="mt-6 bg-gray-200 p-4 rounded-lg flex items-center space-x-4 text-black">
            <img
              src={userData.avatar_url}
              alt="Avatar"
              className="w-24 h-24 rounded-full border-4 border-blue-500"
            />
            <CardContent className="flex flex-col">
              <h2 className="text-xl font-semibold text-blue-600">{userData.name}</h2>
              <p className="text-gray-700">{userData.bio || "Sem biografia."}</p>
            </CardContent>
          </Card>
        )}
      </div>
    </div>
  );
};

export default GitHubProfileSearch;
