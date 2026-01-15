<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>QuizMaster | Pattao National High School</title>

<!-- Tailwind -->
<script src="https://cdn.tailwindcss.com"></script>

<!-- Font -->
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">

<style>
body{
  font-family:'Inter',sans-serif;
  background:linear-gradient(180deg,#fffaf7,#ffffff);
}
.correct{color:green;font-weight:bold;}
.incorrect{color:red;font-weight:bold;}
</style>
</head>

<body class="max-w-3xl mx-auto p-6">

<h1 class="text-3xl font-bold text-red-800 mb-6">
QuizMaster – Pattao National High School
</h1>

<!-- Teacher / Subject -->
<div class="grid grid-cols-1 md:grid-cols-2 gap-4 mb-6">
  <div>
    <label class="font-semibold">Teacher Name</label>
    <input id="teacherName" class="w-full p-2 border rounded" placeholder="Enter teacher name">
  </div>
  <div>
    <label class="font-semibold">Subject</label>
    <input id="subject" class="w-full p-2 border rounded" placeholder="Enter subject">
  </div>
</div>

<!-- Student -->
<label class="font-semibold">Student Name</label>
<input id="studentName" class="w-full p-2 border rounded mb-6" placeholder="Enter student name">

<!-- Quiz -->
<form id="quizForm" class="space-y-6"></form>

<button type="button" onclick="submitQuiz()" class="mt-6 bg-red-700 text-white px-6 py-3 rounded hover:bg-red-800">
Submit Quiz
</button>

<!-- Result -->
<div id="result" class="mt-8 text-lg font-semibold"></div>

<footer class="text-center mt-10 text-gray-500 text-sm">
© 2025 Reynaldo P. Usigan, Mylene A. Baquiran, Rain Bilag
</footer>

<script>
const questions = [
 {q:"What is the basic unit of life?", options:["Atom","Molecule","Cell","Tissue"], answer:2},
 {q:"Which organelle is the powerhouse of the cell?", options:["Nucleus","Mitochondria","Ribosome","Endoplasmic Reticulum"], answer:1},
 {q:"What process do plants use to make food?", options:["Respiration","Photosynthesis","Transpiration","Fermentation"], answer:1},
 {q:"What molecule carries genetic information?", options:["RNA","DNA","Protein","Lipid"], answer:1},
 {q:"Which blood vessels carry blood away from the heart?", options:["Veins","Arteries","Capillaries","Nerves"], answer:1},
 {q:"Which part of the brain controls balance?", options:["Cerebrum","Cerebellum","Medulla","Hypothalamus"], answer:1},
 {q:"Mitosis produces what kind of cells?", options:["Gametes","Identical cells","Different cells","Haploid cells"], answer:1},
 {q:"Which macromolecule stores long-term energy?", options:["Protein","Lipid","Carbohydrate","Nucleic Acid"], answer:1},
 {q:"Animals that eat plants and animals are called?", options:["Herbivores","Carnivores","Omnivores","Detritivores"], answer:2},
 {q:"What gas is exhaled during respiration?", options:["Oxygen","Nitrogen","Carbon Dioxide","Hydrogen"], answer:2}
];

const quizForm = document.getElementById("quizForm");

// Display questions
questions.forEach((q,i)=>{
  const div=document.createElement("div");
  div.innerHTML=`
    <p class="font-semibold mb-2">${i+1}. ${q.q}</p>
    ${q.options.map((opt,idx)=>`
      <label class="block">
        <input type="radio" name="q${i}" value="${idx}"> ${opt}
      </label>`).join("")}
  `;
  quizForm.appendChild(div);
});

function submitQuiz(){
  let score=0;
  let review="";

  questions.forEach((q,i)=>{
    const selected=document.querySelector(`input[name="q${i}"]:checked`);
    if(selected && Number(selected.value)===q.answer){
      score++;
      review+=`<p>${i+1}. <span class="correct">Correct</span></p>`;
    }else{
      review+=`<p>${i+1}. <span class="incorrect">Incorrect</span></p>`;
    }
  });

  const percent=((score/questions.length)*100).toFixed(2);

  const data={
    student:studentName.value,
    teacher:teacherName.value,
    subject:subject.value,
    score:score,
    percentage:percent,
    date:new Date().toLocaleString()
  };

  localStorage.setItem("QuizMasterResult",JSON.stringify(data));

  document.getElementById("result").innerHTML=`
    <p>Student: <strong>${data.student}</strong></p>
    <p>Teacher: <strong>${data.teacher}</strong></p>
    <p>Subject: <strong>${data.subject}</strong></p>
    <p class="mt-2">Score: <strong>${score}/10</strong></p>
    <p>Percentage: <strong>${percent}%</strong></p>
    <hr class="my-4">
    ${review}
  `;
}
</script>

</body>
</html>
