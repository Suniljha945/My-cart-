import { useState, useEffect } from "react"; import { Card, CardContent } from "@/components/ui/card"; import { Button } from "@/components/ui/button"; import { motion } from "framer-motion";

export default function GitHubDashboard() { const [user, setUser] = useState(null); const [repos, setRepos] = useState([]); const username = "octocat"; // change this to your GitHub username

useEffect(() => { async function fetchData() { const userRes = await fetch(https://api.github.com/users/${username}); const userData = await userRes.json(); setUser(userData);

const repoRes = await fetch(
    `https://api.github.com/users/${username}/repos?sort=updated&per_page=6`
  );
  const repoData = await repoRes.json();
  setRepos(repoData);
}
fetchData();

}, []);

return ( <div className="min-h-screen bg-gray-100 p-6"> <div className="max-w-5xl mx-auto space-y-6"> {/* Profile Section */} {user && ( <motion.div initial={{ opacity: 0, y: -20 }} animate={{ opacity: 1, y: 0 }} className="flex items-center space-x-4" > <img
src={user.avatar_url}
alt="avatar"
className="w-20 h-20 rounded-full shadow-md"
/> <div> <h1 className="text-2xl font-bold">{user.name}</h1> <p className="text-gray-600">@{user.login}</p> <p className="text-sm mt-1">{user.bio}</p> <p className="text-sm mt-1">Followers: {user.followers} | Following: {user.following}</p> </div> </motion.div> )}

{/* Repositories Section */}
    <div>
      <h2 className="text-xl font-semibold mb-4">Recent Repositories</h2>
      <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4">
        {repos.map((repo) => (
          <motion.div
            key={repo.id}
            initial={{ opacity: 0, y: 20 }}
            animate={{ opacity: 1, y: 0 }}
            transition={{ duration: 0.3 }}
          >
            <Card className="shadow-md hover:shadow-lg transition">
              <CardContent className="p-4">
                <h3 className="font-semibold text-lg mb-2">{repo.name}</h3>
                <p className="text-sm text-gray-600 line-clamp-2">{repo.description}</p>
                <div className="flex justify-between items-center mt-3">
                  <span className="text-xs text-gray-500">⭐ {repo.stargazers_count}</span>
                  <a
                    href={repo.html_url}
                    target="_blank"
                    rel="noopener noreferrer"
                  >
                    <Button size="sm">View</Button>
                  </a>
                </div>
              </CardContent>
            </Card>
          </motion.div>
        ))}
      </div>
    </div>
  </div>
</div>

); }

