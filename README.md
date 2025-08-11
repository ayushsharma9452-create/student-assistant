# student-assistant
An AI-powered Student Assistant project for Puch AI Hackathon, built with Python. Helps students with queries, learning support, and productivity tools.
student-assistant/
│── main.py
│── database.py
│── models.py
│── routes.py
│── requirements.txt
│── README.md
│── .env.example
from fastapi import FastAPI
from routes import router

app = FastAPI(
    title="Puch AI Hackathon - Student Assistant",
    description="AI-powered Student Assistant for learning and productivity",
    version="1.0.0"
)

app.include_router(router)

@app.get("/")
def home():
    return {"message": "Welcome to Puch AI Hackathon - Student Assistant!"}
    from sqlalchemy import create_engine
from sqlalchemy.ext.declarative import declarative_base
from sqlalchemy.orm import sessionmaker

DATABASE_URL = "sqlite:///./student.db"

engine = create_engine(DATABASE_URL, connect_args={"check_same_thread": False})
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()from sqlalchemy import Column, Integer, String, Text
from database import Base

class Question(Base):
    __tablename__ = "questions"

    id = Column(Integer, primary_key=True, index=True)
    question = Column(Text, nullable=False)
    answer = Column(Text, nullable=True)from fastapi import APIRouter, Depends
from sqlalchemy.orm import Session
from database import SessionLocal, engine
from models import Base, Question

router = APIRouter()

# Create tables
Base.metadata.create_all(bind=engine)

def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

@router.get("/help")
def help_guide():
    return {"guide": "Ask a question using POST /question"}

@router.post("/question")
def ask_question(question: str, db: Session = Depends(get_db)):
    q = Question(question=question, answer="This is a sample AI answer")
    db.add(q)
    db.commit()
    db.refresh(q)
    return {"id": q.id, "question": q.question, "answer": q.answer}fastapi
uvicorn
sqlalchemy
pydantic # For future use - WhatsApp Cloud API / OpenAI keys
API_KEY=your_api_key_here
# 🎓 Puch AI Hackathon – Student Assistant

An AI-powered Student Assistant built with **Python (FastAPI)** for the **Puch AI Hackathon**.  
It helps students with queries, learning support, and productivity tools — all in one place.

---

## 🚀 Features
- 📚 **Study Assistance** – Ask questions and get instant answers.
- 📝 **Notes & Reminders** – Save important topics and deadlines.
- 🔍 **Search Tool** – Quick access to educational resources.
- 🌐 **Future Ready** – Easily connect to WhatsApp Cloud API.

---

## 🛠️ Tech Stack
- **Backend:** Python + FastAPI
- **Database:** SQLite (Local)
- **Future API Integration:** WhatsApp Cloud API
- **Deployment:** Heroku / Render / Railway

---

## 📂 Project Structure
