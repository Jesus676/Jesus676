import React, { useState } from 'react';
import { Camera, Heart, MessageCircle, Send, Bookmark } from 'lucide-react';

// Datos de ejemplo para publicaciones
const initialPosts = [
  {
    id: 1,
    username: 'usuario_ejemplo',
    profilePic: '/api/placeholder/50/50',
    image: '/api/placeholder/400/400',
    likes: 125,
    caption: 'Mi primera publicación en esta app 🚀',
    comments: [
      { user: 'amigo1', text: '¡Qué genial!' },
      { user: 'amigo2', text: 'Me encanta' }
    ]
  },
  {
    id: 2,
    username: 'otra_cuenta',
    profilePic: '/api/placeholder/50/50',
    image: '/api/placeholder/400/400',
    likes: 87,
    caption: 'Día hermoso hoy',
    comments: [
      { user: 'seguidor3', text: 'Foto increíble' }
    ]
  }
];

const InstagramClone = () => {
  const [posts, setPosts] = useState(initialPosts);
  const [newPost, setNewPost] = useState('');

  const handleLike = (postId) => {
    setPosts(posts.map(post => 
      post.id === postId 
        ? {...post, likes: post.likes + 1} 
        : post
    ));
  };

  const handlePostUpload = () => {
    if (newPost.trim()) {
      const newPostObj = {
        id: posts.length + 1,
        username: 'usuario_actual',
        profilePic: '/api/placeholder/50/50',
        image: '/api/placeholder/400/400',
        likes: 0,
        caption: newPost,
        comments: []
      };
      setPosts([newPostObj, ...posts]);
      setNewPost('');
    }
  };

  return (
    <div className="max-w-md mx-auto bg-white min-h-screen">
      {/* Barra superior */}
      <header className="flex justify-between items-center p-4 border-b">
        <h1 className="text-2xl font-bold">Instagram Clone</h1>
        <Camera className="text-black" />
      </header>

      {/* Área de subir publicación */}
      <div className="p-4 flex">
        <input 
          type="text" 
          value={newPost}
          onChange={(e) => setNewPost(e.target.value)}
          placeholder="Escribe una nueva publicación"
          className="flex-grow border p-2 mr-2"
        />
        <button 
          onClick={handlePostUpload}
          className="bg-blue-500 text-white px-4 py-2 rounded"
        >
          Publicar
        </button>
      </div>

      {/* Feed de publicaciones */}
      {posts.map(post => (
        <div key={post.id} className="border-b pb-4 mb-4">
          {/* Encabezado de publicación */}
          <div className="flex items-center p-4">
            <img 
              src={post.profilePic} 
              alt={post.username} 
              className="w-10 h-10 rounded-full mr-3"
            />
            <span className="font-bold">{post.username}</span>
          </div>

          {/* Imagen de publicación */}
          <img 
            src={post.image} 
            alt="Publicación" 
            className="w-full"
          />

          {/* Acciones de publicación */}
          <div className="flex justify-between p-4">
            <div className="flex space-x-4">
              <button onClick={() => handleLike(post.id)}>
                <Heart className={`text-${post.likes > 0 ? 'red' : 'black'}-500`} />
              </button>
              <MessageCircle />
              <Send />
            </div>
            <Bookmark />
          </div>

          {/* Likes y descripción */}
          <div className="px-4">
            <p className="font-bold">{post.likes} Me gusta</p>
            <p>
              <span className="font-bold mr-2">{post.username}</span>
              {post.caption}
            </p>
            {post.comments.map((comment, index) => (
              <p key={index} className="text-gray-600">
                <span className="font-bold mr-2">{comment.user}</span>
                {comment.text}
              </p>
            ))}
          </div>
        </div>
      ))}
    </div>
  );
};

export default InstagramClone;
