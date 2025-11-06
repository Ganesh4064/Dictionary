function solution(D) {
  const dayNames = ['Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat', 'Sun']
  const week = Array(7).fill(null)

  const dateToIndex = (dateStr) => {
    const d = new Date(dateStr + 'T00:00:00')
    const jsDay = d.getDay()
    return (jsDay + 6) % 7
  }

  for (const dateStr in D) {
    const val = Number(D[dateStr])
    const idx = dateToIndex(dateStr)
    if (week[idx] === null) week[idx] = val
    else week[idx] += val
  }

  if (week[0] === null || week[6] === null) return {}

  let i = 0
  while (i < 7) {
    if (week[i] !== null) {
      i++
      continue
    }
    let left = i - 1
    while (left >= 0 && week[left] === null) left--
    let right = i
    while (right < 7 && week[right] === null) right++
    const gap = right - left
    const leftVal = week[left]
    const rightVal = week[right]
    const step = (rightVal - leftVal) / gap
    for (let k = 1; k < gap; k++) {
      week[left + k] = Math.round(leftVal + step * k)
    }
    i = right + 1
  }

  const result = {}
  for (let idx = 0; idx < 7; idx++) result[dayNames[idx]] = week[idx]
  return result
}

